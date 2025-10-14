%% FGSM Adversarial Attack Demo - MATLAB

% Load pretrained ResNet-18 and convert to dlnetwork
netSeries = resnet18;
lgraph = layerGraph(netSeries);
lgraph = removeLayers(lgraph, 'ClassificationLayer_predictions'); 
% Remove classification layer
net = dlnetwork(lgraph);  
% Now suitable for gradient calculations

% Read and preprocess input image
img = imread('peppers.png');                     
 % Use a sample image
img = imresize(img, [224 224]);                   
% Resize to input size
img = single(img) / 255;                          
% Normalize to [0,1]
dlX = dlarray(img, 'SSC');                        
% Convert to dlarray (Spatial-Spatial-Channel)

% Get original prediction
scores = forward(net, dlX);                       
% Raw logits
[~, labelIdx] = max(extractdata(scores));         
% Original predicted class
classNames = netSeries.Layers(end).Classes;       
% Original class labels
disp("Original Prediction: " + string(classNames(labelIdx)));

% Perform FGSM attack
epsilon = 0.02;  % Small perturbation value
[grad, ~] = dlfeval(@(X) modelGrad(net, X, labelIdx), dlX);   
% Compute gradient of loss w.r.t. input
noise = epsilon * sign(extractdata(grad));                    
% Create FGSM noise
advImg = img + noise;                                         
% Add perturbation
advImg = max(min(advImg, 1), 0);                              
% Clip to [0,1]

% Predict again on adversarial image
dlAdvX = dlarray(advImg, 'SSC');
advScores = forward(net, dlAdvX);
[~, advIdx] = max(extractdata(advScores));

% Display predictions
disp("Adversarial Prediction: " + string(classNames(advIdx)));

% Show original and adversarial images
figure;
subplot(1,3,1); imshow(img); title("Original Image");
subplot(1,3,2); imshow(noise, []); title("Adversarial Noise");
subplot(1,3,3); imshow(advImg); title("Adversarial Image");

%% Helper Function
function [grad, loss] = modelGrad(net, dlX, labelIdx)
    scores = forward(net, dlX);
    target = zeros(size(scores), 'like', scores);
    target(labelIdx) = 1;
    loss = crossentropy(scores, target);
    grad = dlgradient(loss, dlX);
end
