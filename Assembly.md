# Getting to Know Assembly

## [An Introduction](https://ggbaker.ca/295/content/assembly.html)

* See [Assembly Cheat Sheet](https://ggbaker.ca/295/x86.html) for the types of registers.
For this course, we will assume that we are working with 64-bit computers and use X86-64 syntax for Assembly code.

##### Compiler Stuff

`gcc -Wall -Wpedantic -std=c17 -march=haswell hello.c -o hello`

`gcc` - the compiler we use
`gcc { stuff } hello.c` -   those 'stuff' are flags; specific things we want the compiler should look out 
                            for when we compile
`-g` - flag to turn on debugger
`-c` - flag to tell the compiler to COMPILE ONLY INTO `.o` from the `.c` files that follow.
All files need to be turned into `.o` object files (machine code of 0s and 1s) before it can run.

        C/C++ -----> Assembly --> machine code (.o)

Jave/C#/Python ----> Bytecode --> interpreter -> machine code

We can turn codes written in other languages into machine code (.o) -> link them -> run them as part of a bigger program
We can expect Assembly to be slightly faster than C b/c its 'one step closer' to machine code than C ![see diagram](https://ggbaker.ca/295/media/running-code.svg)

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
```
We use preserve registers to store values that we want to keep throught our code, 
EVEN AFTER we call another function.  We should've stored `A+1` in any non-special preserved
registers above.

```
My Code: // f(A) -> return A + 1

A + 1 -> reg12  // store A + 1 in the preserved register: 'r12'
call func_f(A)
___________ function f body _____________
        B -> reg12 // store B in A

        reg12 holds B until the end of function call.
        At the end of f() function call, f() stack will 
        RESTORE THE ORIGINAL VALUE OF REG_12 -> A + 1
_________________________________________

return ?? // since reg12 still holds A+1 after f() is called, we can 
             return it.
```
NOTE: the value inside `rax` is what gets returned.  we need to
`set rax = r12` to return A + 1 in our code.

#### Special Registers & Stack Structure
`rax` - the return register; when we hit `ret` or return, the stuff inside 
this register is what gets returned.

`rdi` - the register that holds the `first parameters value` in a function call

`rsi` - the register that . . .     `second parameters value` . . .
        eg) f(x,y) { do something } => x will be stored in `rdi` 
                                       y will be stored in `rsi`

`rdx` -            . . .            `third `        . . .

`rcx` -            . . .            `fourth`        . . .

`r8` -             . . .            `fifth`         . . .

`r9` -             . . .            `sixth`         . . .
        eg) f(x,y,z, . . .)

##### Preserved Registers
`rsp` - the register holding the pointer to the stack of the function.  It `ALWAYS` points
to the top of the stack by the nature of the STACK LIFO (last in - first out) structure

In theory, we can move this pointer by ADDING/SUBTRACTING from the register but we 
should not as this destroys the philosophy of the stack data structure.

|     | <- %rsp     |     |

|     |             |     |

|     |             |     | <- %rsp after subtracting we move 'down' the stack

|_____|             |_____|         and get the data at this location.

We can also `push` a value into the stack -> change it -> and `pop` the stack 
when we want the original value back.

``` Ex)
push X //ie) X = 3 will get pushed into the stack
X = X = 10
pop X // restore X to 3

return X <- return 3
```

`rbp` - 

`rbx` - 

IDK whats so special about these two...

