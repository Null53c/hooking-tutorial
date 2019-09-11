# Hooking Tutorial
The different ways of hooking (MASM sythax)

Byte patching in the .text section is the easiest and most common way to place a hook.
Hooking libraries like Microsoft Detours (Download) are used alot.<br>

There are various ways to redirect the code flow. You can place a normal JMP instruction (5 bytes in size) or try some hotpatching using a short JMP (2 bytes in size) to some location where is more space for a 5 byte JMP.<br>

You can place a CALL instruction which works same as a JMP but pushes the returnaddress on the stack before jumping. You can also just push the address on the stack and then call RETN which jumps to the last adddress on stack and therefore behaves like a JMP.


### IAT / EAT Hooking
This hooking method is based on how the PE files are working on windows.
It means "Import/Export Address Table". This address table contains the pointer to the APIs and is adjusted by the PE loader when the file is executed.<br>

You can either loop the whole table and search for a function and redirect it or you can find it manually using OllyDbg or IDA.
The basic idea is that you replace a certain API with your hooked function. Thats not only good for simple API hooking but it can also be used for a DirectX hook<br>
```c
pEnterCriticalSection = *(EnterCriticalSection_t*)hCriticalSection;
*(DWORD*)hCriticalSection = (DWORD)nEnterCriticalSection;
```
As you can see its a simple pointer redirection. The funny thing is that this old method is still undetected for some anticheats.

# To be continued
