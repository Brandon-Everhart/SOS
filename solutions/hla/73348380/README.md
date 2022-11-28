***ENVIRONMENT***

- HLA (High Level Assembler - HLABE back end, LD linker)
Version 2.16 build 4409 (prototype)
- Ubuntu 21.04

***NOTE***

- HLA provides higher level capabilities which are helpful to layout and test the desired functionality before transitioning to lower level assembly concepts. The following example provides a working solution using the higher level functionality of HLA which provides good starting point to work towards assembly. 
- Assembly code is on average less human readable than higher level languages so it is best practice to frequently use comments throughout your code. 
- HLA also allows for indentation which further improves code readability. 
    
***EXAMPLE***

```hla
program Strlen;
#include("stdlib.hhf")

storage
    inputStr: string;

static 
    inputLen: int32;

begin Strlen;
    // Prompt user for input
    stdout.put("Feed me: ");

    // Flush Input
    stdin.flushInput();

    // Allocate and read input
    stdin.a_gets();

    // Save result in inputStr
    mov(EAX, inputStr);

    // Display input back to user
    stdout.put("The String You Entered: ", inputStr, nl);

    // Calculate length of input string
    str.length(inputStr);

    // Save result in inputLen
    mov(EAX, inputLen);

    // Display input length to the user
    stdout.put("Has Length = ", inputLen, nl);

    // Free allocated memory
    str.free(inputStr);
end Strlen;
```