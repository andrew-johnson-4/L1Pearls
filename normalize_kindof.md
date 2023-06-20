## Kinding Algorithm

```lsts
type KindOf = Diverges(Integer) | Converges(Integer) | Undecided(Integer); 

let kind_of(x: Integer): KindOf = {
   //The kind system runs in a constant space abstract computer
   //Programs here are constant space, but not constant time
   //The abstract computer is designed to traverse the AST to get whatever it needs
   //The goal of running a program in the kind system is to gain information at the type level
   //Kind rules are defined in the AST (the kind system is extensible)

   ldr x         //input rule and arguments:       "normalize x"
   mov $(x) $$x  //destructure rule and arguments: "propagate the information in the namespace of x into the type of x"
   label $$$?    //apply rule and arguments:       "apply previously normalized rule and store here"
   jmp $$$!      //output rule and arguments:      "ask whether previously normalized rule diverges"

   //The instruction set for this abstract computer cannot determine the decidability of all decidability problems
   //The instruction set for this abstract computer can implement all strongly normalizing computable functions
   //The abstract computer itself is not strongly normalizing (strange input gives strange output)
}
```

This rank level normalization is the basis of the "flexible soundness" guarantees in LSTS.
Basically this compartmentalization permits a more haphazard approach to development and deployment of new type systems.
Creating sound type systems is still desireable, but now there are decidability guiderails during the development of new stuff.

There are many ways to approach rank level decidability, but it is at most only memorization.
The simplest decidability check will return Undecided, and never provide any information.
Anything more than that is rote memorization of rule-dependent logic from the kinding system.

## Kinding Instruction Set

```lsts
type Instr = Integer
           | Instr !
           | Instr ?
           | Instr $$$ Instr
           | label Instr
           | jmp Instr
           | $$$!
           | $$$?
           | $$Integer
           | $(Integer)
           | mov Instr Instr
           | ldr Instr
```
