# UrbanBishopLocal

![UrbanBishopLocal](https://raw.githubusercontent.com/slyd0g/UrbanBishopLocal/master/example.png)

## Description

A port of [UrbanBishop](https://github.com/FuzzySecurity/Sharp-Suite#urbanbishop) project for inline shellcode execution. The execution vector uses a delegate vs an APC on a suspended threat at ntdll!RtlExitUserThread in UrbanBishop

I've created this fork because it was very unpleasent, to change source file whenever i tried different approach with the shellcode or XOR key. Im not a c# dev at all, feel free to fork and modify this version as you wish.

Supposed to be used on a local machine for a real-time testing, for compiled version check-out other branch.

- `NtCreateSection` is used to create a section object
- `NtMapViewOfSection` creates a section view with RW permissions we can write shellcode to
- Shellcode is written to the section view
- A second call to `NtMapViewOfSection` creates a section view with RX permissions
- A pointer to the base address of the shellcode is converted to a delegate and executed

## Usage

1. Base64 encode XOR encrypted 64 bit shellcode with PowerShell
   - `[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("$PSScriptRoot\encrypted_shellcode.bin")) | clip`
2. Pass Base64 shellcode into the program console with the -p || --path <path_to_file_with_base64_shellcode>
3. Pass XOR key into the program console with the -k || --key <encryption/decryption_password>
4. Build the project for x64
