Task 5: Analyze DLL File using IDA Pro
Aim:
To analyze the internal structure and exported functions of a Windows DLL file using IDA Pro.
Procedure:
1. Open IDA Pro.
2. Load the DLL file:
o Choose "New File".
o Select the .dll file.
o IDA auto-detects architecture (confirm 32-bit/64-bit).
3. Allow auto-analysis:
o IDA analyzes functions, imports, and exports.
4. Review exports:
o View > Open Subviews > Exports
5. Explore disassembled functions:
o Use function view and graph view (Spacebar) to inspect logic.
6. Annotate code:
o Add comments or rename functions for better readability.
Example Output:
Let’s assume the DLL has the following exported function:
asm
CopyEdit
Exported function: AddNumbers
.text:10001000 AddNumbers proc near
.text:10001000 push ebp
.text:10001001 mov ebp, esp
.text:10001003 mov eax, [ebp+8]
.text:10001006 add eax, [ebp+0Ch]
.text:10001009 pop ebp
.text:1000100A retn
.text:1000100A AddNumbers endp
