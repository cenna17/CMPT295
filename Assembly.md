# Getting to Know Assembly

## [An Introduction](https://ggbaker.ca/295/content/assembly.html)

* See [Assembly Cheat Sheet](https://ggbaker.ca/295/x86.html) for the types of registers.
For this course, we will assume that we are working with 64-bit computers and use X86-64 syntax for Assembly code.

##### Assembly Template
 ```bash
    .section .note.GNU-stack, ""
        .global add10
        .text

    {code body}

    ret
```

### Registers [(List of Registers)](https://ggbaker.ca/295/x86.html)

* Registers are used to store data and may be split into multiple categories.
* `Preserved Registers` may be used to store values that we want to preserve for later use.

%r12    %rbx <---> The second column here
%r13    %rbp <---> are also preserved registers
%r14    %rsp <---> but they are special.
%r15

are preserved registers. 

* There are only so many registers !
If our code calls another function in Assembly, the OTHER FUNCTION may use one of those registers `=>` changing its value
`Example`
``` 
My Code: // f(A) -> return A + 1

    reg_A -> A + 1 //store A + 1 in reg_A
    call func_f(A)
        ___________ function f body _____________
        B = reg_A // stores B in A

        reg_A now contains B NOT value A+1 from my code
        so, I have lost that value.  
        _________________________________________

    return ?? // Therefore, I've lost the calc for A+1

We use preserve registers to store values that we want to keep throught our code, 
EVEN AFTER we call another function. 



