***ENVIRONMENT***

- HLA (High Level Assembler - HLABE back end, LD linker)
Version 2.16 build 4409 (prototype)
- Ubuntu 20.04.1
 
***SOLUTION***

- The problem is `maxVal=input;` is an invalid statement in HLA. To correct this problem us the `mov` instruction as follows: `mov(input, maxVal);`. 
- However, after this correction you will see the following error:

```bash
Assembling "src.hla" to "src.o"
Error in file "src.hla" at line 27 [errid:102032/hlaparse.bsn]:
Memory to memory comparisons are illegal.
Near: << ) >>

hlaparse: oututils.c:2480: FreeOperand: Assertion `o->l.leftOperand != ((void *)0)' failed.
Aborted (core dumped)
```

- This is because HLA does not support comparison between memory objects. One solution is to store the user entered number in `EAX` and change all occurrences of the `input` variable to `EAX`.

***EXAMPLE***

```hla
program MaxMin;
#include("stdlib.hhf")

storage
    maxVal: int32; 
    minVal: int32;

static
    count:   int32:=   0;
    sum:     int32:=   0;
    boolVar: boolean:= true;

begin MaxMin;
    while(boolVar) do
        stdout.put("Enter a number, 0 to stop: ");
        stdin.geti32();

        if (EAX = 0 ) then
            break;
        elseif ( count = 0 ) then
            mov(EAX, maxVal);
            mov(EAX, minVal);
        elseif (EAX > maxVal) then
            mov(EAX, maxVal);
        elseif (EAX < minVal) then
            mov(EAX, minVal);
        endif;
    
        add(EAX, sum);
        add(1, count);   
    endwhile;

    stdout.newln();
    stdout.put("Total: ",sum,nl,"Count: ",count,nl,"Maximum: ",maxVal,nl,"Minimum: ",minVal,nl);
end MaxMin;
```