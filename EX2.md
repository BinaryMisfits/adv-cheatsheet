% FGSM Adversarial Attack on MNIST - Complete MATLAB Code
clear; clc; close all;

%% Load MNIST dataset
digitDatasetPath = fullfile(matlabroot,'toolbox','nnet','nndemos','nndatasets','DigitDataset');
imds = imageDatastore(digitDatasetPath, ...
    'IncludeSubfolders',true,'LabelSource','foldernames');

% Resize and split data
imds.ReadFcn = @(filename) imresize(imread(filename), [28,28]);
[imdsTrain, imdsTest] = splitEachLabel(imds, 0.8);

%% Define a simple CNN
layers = [
    imageInputLayer([28 28 1],"Name","input")
    convolution2dLayer(3,8,"Padding","same","Name","conv1")
    batchNormalizationLayer("Name","bn1")
    reluLayer("Name","relu1")
    maxPooling2dLayer(2,"Stride",2,"Name","maxpool1")
    
    convolution2dLayer(3,16,"Padding","same","Name","conv2")
    batchNormalizationLayer("Name","bn2")
    reluLayer("Name","relu2")
    fullyConnectedLayer(10,"Name","fc")
    softmaxLayer("Name","softmax")
    classificationLayer("Name","output")];

%% Convert to layerGraph and remove classification layer
lgraph = layerGraph(layers);
lgraph = removeLayers(lgraph, 'output');       % Remove classification layer
lgraph = removeLayers(lgraph, 'softmax');      % Remove softmax layer too

% Create dlnetwork
dlnet = dlnetwork(lgraph);

%% Load one test image
img = readimage(imdsTest, 1);
label = imdsTest.Labels(1);
X = im2single(img);           % Convert to single precision
dlX = dlarray(X, 'SSC');      % 'SSC' = spatial, spatial, channel

%% Forward pass to get true label index
scores = predict(dlnet, dlX);
[~, trueLabelIdx] = max(extractdata(scores));

%% One-hot encode the true label
T = zeros(10,1,'single');
T(trueLabelIdx) = 1;
dlT = dlarray(T);

%% Compute loss and gradient using dlfeval
[loss, grad] = dlfeval(@modelGradients, dlnet, dlX, dlT);

%% FGSM Attack
epsilon = 0.1;
advX = dlX + epsilon * sign(grad);

%% Predict the adversarial image
advScores = predict(dlnet, advX);
[~, advLabelIdx] = max(extractdata(advScores));

%% Show original and adversarial images
figure;
subplot(1,2,1)
imshow(X)
title(["Original", "Predicted: " + string(trueLabelIdx-1)])

subplot(1,2,2)
imshow(extractdata(advX))
title(["Adversarial", "Predicted: " + string(advLabelIdx-1)])

%% Function to compute loss and gradient
function [loss, gradients] = modelGradients(dlnet, dlX, dlT)
    scores = forward(dlnet, dlX);
    loss = crossentropy(scores, dlT);
    gradients = dlgradient(loss, dlX);
end
