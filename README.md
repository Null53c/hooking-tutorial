# Hooking Tutorial
---The different ways of hooking (MASM sythax)---<br>


Hooking means basically that you interrupt a function call and redirect it to your function.  
Lets make a small example:  
A Game has a function called ReduceHP();  
and it looks something like this  
```c
ReduceHP() {
hp -= 15;
}
```
it gets called every time you get damaged in the game (e.g. shot).  
If you hook this function, the game will still be able to call ReduceHP() but instead of their legitimate function, the call will be redirected to your function (thats why it only works on internal hacks, since you have to use gamefunctions).
If you hook this function, you could rewrite it like this  
```c
educeHP() {
hp += 15;
ReduceHP(); <--- Calling the REAL ReduceHP() function, to ensure the flow of the game won't change.
}
```
What did we do? Instead of reducing the hp by 15, we add 15 to our current hp and THEN call the real reduceHP function. Even though the hp will be reduced by 15 in the real function, you won't lose any HP since you added 15 in your function.  

In other words:   <br><br>
Hooking allows you to intercept precise branches of execution and redirect them to injected code that you’ve written to dictate what the game should do next, and it comes in a variety of flavors.  
* Call Hooking ( A call hook directly modifies the target of a CALL operation to point to a new piece of code ).
* VF Table Hooking ( Virtual function (VF) table hooks don’t modify assembly code. Instead, they modify the function addresses stored in the VF tables of classes ).
* IAT Hooking ( IAT hooks actually replace function addresses in a specific type of VF table, called the import address table (IAT). Each loaded module in a process con-tains an IAT in its PE header ).
* Jump Hooking ( Jump hooking allows you to hook code in places where there is no branch-ing code to manipulate. A jump hook replaces the code being hooked with an unconditional jump to a trampoline function ).



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
