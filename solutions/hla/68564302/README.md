***ENVIRONMENT***

- HLA (High Level Assembler - HLABE back end, LD linker)
Version 2.16 build 4463 (prototype)
- Ubuntu 20.10

***SOLUTION***

- Five values are pushed to the stack when calling `theSame()`, those are the `return address`, `w`, `x`, `y`, and `z`. However in the prologue for `theSame()` six values are currently popped off of the stack. 

```hla
begin theSame;
    pop (returnAddress);
    pop (temp);
    pop (z);
    pop (y);
    pop (x);
    pop (w);
```
- The additional `pop (temp);` seen above causes the `temp` variable to store the value passed in for `z`, `z` to store the value passed in for `y`, `y` to store the value passed in for `x`, `x` to store the value passed in for `w`, and `w` to store the next random value on the stack. Removing the `pop (temp);` instruction will result in your example case passing.

```
Feed Me W: 2
Feed Me X: 2
Feed Me Y: 2
Feed Me Z: 1
Not the same. AL = 0
```

- However the following case will still fail due to errors in the comparison logic. 

```
Feed Me W: 1
Feed Me X: 2
Feed Me Y: 2
Feed Me Z: 2
Same. AL = 1
```

- In the following code if `z`, `y`, and `x` are equal then the code will jump to `ReturnOne` with out checking the value of `w`. This is corrected by removing the first `je ReturnOne;` and changing the first `jmp ReturnZero;` to `jne ReturnZero;`.

```hla
mov (z, BX);
cmp (y, BX);                // Compare z & y
jne ReturnZero;
    
mov (y, BX);
cmp (x, BX);                // Compare y & x
je ReturnOne;
jmp ReturnZero;
    
mov (x, BX);
cmp (w, BX);                // Compare x & w
je ReturnOne;
jmp ReturnZero;
```

***EXAMPLE***

```hla
program Same;
#include("stdlib.hhf");

procedure theSame(w: int16; x: int16; y: int16; z: int16); @nodisplay; @noframe;  
begin theSame;
    pop(EDX);          // Return Address
    pop(z);
    pop(y);
    pop(x);
    pop(w);
    push(EDX);         // Return Address
    mov(z, BX);
    cmp(y, BX);        // Compare z & y
    jne ReturnZero;
    mov(y, BX);
    cmp(x, BX);        // Compare y & x
    jne ReturnZero;
    mov(x, BX);
    cmp(w, BX);        // Compare x & w
    jne ReturnZero;
    mov(1, AL);
    jmp ExitSequence;
    ReturnZero:
        mov(0, AL);
    ExitSequence:
        ret();
end theSame;

begin Same;
    stdout.put("Feed Me W: ");
    stdin.geti16();
    push(AX);
    stdout.put("Feed Me X: ");
    stdin.geti16();
    push(AX);
    stdout.put("Feed Me Y: ");
    stdin.geti16();
    push(AX);
    stdout.put("Feed Me Z: ");
    stdin.geti16();
    push(AX);
    call theSame;
    cmp(AL, 1);
    jne NumbersAreDifferent;
    stdout.put("Same. AL = 1");
    jmp EndProgram;
    NumbersAreDifferent:
        stdout.put("Not the same. AL = 0");
    EndProgram:
end Same;
```
