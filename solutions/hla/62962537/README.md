***ENVIRONMENT***

- HLA (High Level Assembler - HLABE back end, LD linker)
Version 2.16 build 4409 (prototype)
- Ubuntu 20.04.1

***NOTE***

- As mentioned in previous comments:
    - Nested loops using the same loop control variable is poor design and leads to hard to debug problems such as an infinite loop.
    - This assignment is more simply solved using two sequential loops instead of nested loops.
    - Starting with a higher level language to design the algorithm is good practice. The following example provides a working solution using the higher level functionality of HLA which is a good starting point to work towards assembly. 

***EXAMPLE***

```hla
program Boxit;
#include ("stdlib.hhf");

begin Boxit;
    stdout.put("Gimme a decimal value to use as n: ");
    stdin.geti32();

    for ( xor(EBX, EBX); EBX <= EAX; add(2, EBX) ) do
        stdout.puti32(EBX);
        stdout.put(", ");
    endfor;

    stdout.newln();

    for ( mov(1, EBX); EBX <= EAX; add(2, EBX) ) do
        stdout.puti32(EBX);
        stdout.put(", ");
    endfor;

end Boxit;
```