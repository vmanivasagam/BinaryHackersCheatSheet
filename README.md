# BinaryHackersCheatSheet

# pwntools

1. **checksec \<binary\>**
 
  [*] '/var/challenge/level8/8_real' <br/>
    Arch:     i386-32-little<br/>
    RELRO:    No RELRO<br/>
    Stack:    No canary found<br/>
    NX:       NX disabled<br/>
    PIE:      PIE enabled<br/>
    RWX:      Has RWX segments<br/>

**RELRO** - RELRO stands for Relocation Read-Only, meaning that the headers in your binary, which need to be writable during startup of the application (to allow the dynamic linker to load and link stuff like shared libraries) are marked as read-only when the linker is done doing its magic (but before the application itself is launched). The difference between Partial RELRO and Full RELRO is that the Global Offset Table (and Procedure Linkage Table) which act as kind-of process-specific lookup tables for symbols (names that need to point to locations elsewhere in the application or even in loaded shared libraries) are marked read-only too in the Full RELRO. Downside of this is that lazy binding (only resolving those symbols the first time you hit them, making applications start a bit faster) is not possible anymore. <br/>

**Canary** - A Canary is a certain value put on the stack (memory where function local variables are also stored) and validated before that function is left again. Leaving a function means that the "previous" address (i.e. the location in the application right before the function was called) is retrieved from this stack and jumped to (well, the part right after that address - we do not want an endless loop do we?). If the canary value is not correct, then the stack might have been overwritten / corrupted (for instance by writing more stuff in the local variable than allowed - called buffer overflow) so the application is immediately stopped. <br/>

**NX** - The abbreviation NX stands for non-execute or non-executable segment. It means that the application, when loaded in memory, does not allow any of its segments to be both writable and executable. The idea here is that writable memory should never be executed (as it can be manipulated) and vice versa. Having NX enabled would be good. <br/>

**PIE** - The last abbreviation is PIE, meaning Position Independent Executable. A No PIE application tells the loader which virtual address it should use (and keeps its memory layout quite static). Hence, attacks against this application know up-front how the virtual memory for this application is (partially) organized. Combined with in-kernel ASLR (Address Space Layout Randomization, which Gentoo's hardened-sources of course support) PIE applications have a more diverge memory organization, making attacks that rely on the memory structure more difficult. <br/>

2. **Commands**

In[]: import pwn <br/>
In[]: pwn.context.arch <br/>
Out[]: i386 <br/>
In[]: pwn.context.bits <br/>
Out[]: 32 <br/>
In[]: pwn.context.arch='amd64'    -- Not required for binary assignment <br/>

