#### Nullbyte-DLL-injection-bypass

A method to bypass a Nullbyte in a POP-POP-RETN address for exploiting local SEH overflows via DLL injection

#### What
Have you ever encountered thousands of useless null byte ridden POP POP RETN addresses generated by Mona.py when exploiting an SEH overflow on a Windows-based system? Have you tried using a partial POP POP RETN overwrite that is documented in some exploits? And everything fails. 

![Image of mona ouput](images/pprnull.png)

Well, here is an alternative, via DLL injection, let's add our own DLL module and call a clean and Nullbyte free POP POP RETN address.

This will pretty much only apply to Local SEH overflows because it requires to you inject a DLL into the SEH overflow vulnerable process.

How practical is this? It's really not that practical via the fact that your only exploiting a *local* SEH overflow. But if you are addicted to popping shells and calculators, this is a fun way to bypass a POP POP RETN address that has a Nullbyte in its address simply by injecting our own.

#### Where

![exploitation process](images/process.png)

1. An output payload file is created since this is exploiting a local SEH based buffer overflow, within the output file is a POC exploit for hijacking the SEH handler and using a (special) POP POP RET to escape the next SEH handler in the SEH chain.

2. You need a process PID to inject a DLL into a process, the PID is automatically discovered via the process_injection() function using the psutil Python library for PID discovery.

3. The drop_DLL_disk() function is called which decodes and drops a BASE64 encoded payload DLL, in this case, it's originally essfun.dll from vulnserver. After dropping the DLL payload to disk, the DLL is injected into the vulnerable process via the automatically discovered PID.

4. After the new DLL is injected into the vulnerable running process (SurfOffline Professional), the attacker can exploit the vulnerable input field in the "New project" creation tool in SurfOffline Professional. File > New Program > Project Name > OK. This will use the new POP POP RET from the injected DLL to pop a calc.exe.

#### The exploit

This exploit is taken advantage of a vulnerable input field in the SurfOffline Professional program. It's vulnerable to a local SEH based overflow. 

The injected DLL is "essfunc.dll" from the vulnserver exploit development series.
