# Binary bomb

#### Details
<pre>
A "binary bomb" is a program provided to students as an object code file.
When run, it prompts the user to type in 6 different strings. If any of these is
incorrect, the bomb ``explodes,'' printing an error message and logging the
event on a grading server. Students must ``defuse'' their own unique bomb by
disassembling and reverse engineering the program to determine what the 6
strings should be. The lab teaches students to understand assembly language, and
also forces them to learn how to use a debugger. It's also great fun. A
legendary lab among the CMU undergrads.
</pre>


#### Phase 1
```asm
Dump of assembler code for function main:
   0x08048a4f <+159>:   add    esp,0x20
   0x08048a52 <+162>:   call   0x80491fc <read_line>           ; get line input
   0x08048a57 <+167>:   add    esp,0xfffffff4
   0x08048a5a <+170>:   push   eax                             ; parameter_1
   0x08048a5b <+171>:   call   0x8048b20 <phase_1>             ; phase_1(eax)
   0x08048a60 <+176>:   call   0x804952c <phase_defused>
   
Dump of assembler code for function phase_1:
   0x08048b20 <+0>: push   ebp
   0x08048b21 <+1>: mov    ebp,esp
   0x08048b23 <+3>: sub    esp,0x8
   0x08048b26 <+6>: mov    eax,DWORD PTR [ebp+0x8]             ; eax = *(ebp+0x8) = arg_1 
   0x08048b29 <+9>: add    esp,0xfffffff8
   0x08048b2c <+12>:    push   0x80497c0                       ; parameter 2
   0x08048b31 <+17>:    push   eax                             ; parameter 1
   0x08048b32 <+18>:    call   0x8049030 <strings_not_equal>   ; strings_not_equal(eax,0x80497c0)
   0x08048b37 <+23>:    add    esp,0x10
   0x08048b3a <+26>:    test   eax,eax                         ; set ZF 
   0x08048b3c <+28>:    je     0x8048b43 <phase_1+35>          ; jmp if eax = 0 -+
   0x08048b3e <+30>:    call   0x80494fc <explode_bomb>        ; BOOOOM          |
   0x08048b43 <+35>:    mov    esp,ebp                         ; <---------------+
   0x08048b45 <+37>:    pop    ebp                             
   0x08048b46 <+38>:    ret                                    ; ret to main() we defused it yay \o/ 
End of assembler dump.
```
`strings_not_equal` takes 2 parameters <br>
`push eax` == `push *(ebp+8)` and <br>
`push 0x080497c0` this one seems to be `CONSTANT` <br>
so, after I examine this address. I got
```asm
x/s 0x080497c0
0x80497c0:  "Public speaking is very easy."
```
Look like I have password, <br> 
then try to input with this secret string to `phase_1`
```bash
xelenonz@sandbox:/tmp$ ./bomb
Welcome to my fiendish little bomb. You have 6 phases with
which to blow yourself up. Have a nice day!
Public speaking is very easy.
Phase 1 defused. How about the next one?
```

#### Phase 2
Stay tuned
