## Kinding Algorithm

The kind system runs in a constant-space abstract computer.
Programs here are constant-space, but not constant-time.
The abstract computer is designed to traverse the AST to get whatever it needs.
The goal of running a program in the kind system is to gain information at the type level.
Kind rules are defined in the AST (the kind system is extensible).

```lsts
type KindOf = Diverges(Integer) | Converges(Integer) | Undecided(Integer)

let kind_of(x: Integer): KindOf = {
   ldr x         //input rule and arguments:       "normalize x"
   mov $(x) $$x  //destructure rule and arguments: "propagate the information in the namespace of x into the type of x"
   label $$$?    //apply rule and arguments:       "apply previously normalized rule and store here"
   jmp $$$!      //output rule and arguments:      "ask whether previously normalized rule diverges"
}

//strongly normalizing version, without decidability check
let kind_of_quiet(x: Integer) = {
   ldr x
   mov $(x) $$x
   label $$$?
}
```

The instruction set for this abstract computer *cannot* determine the decidability of all decidability problems.
The instruction set *can* implement all strongly normalizing computable functions.
The abstract computer itself is not strongly normalizing (when `jmp` is included).
Without `jmp` all programs will be strongly normalizing.

## Kinding Instruction Set (RISC)

```lsts
type Instr = Integer           //A λ-calculus term
           | Instr !           //
           | Instr ?           //
           | Instr $$$ Instr   //
           | label Instr       //
           | jmp Instr         //Return β-normal form, may diverge
           | $$$!              //Load previous value
           | $$$?              //Load previous type
           | $$Instr           //
           | $(Instr)          //
           | mov Instr Instr   //
           | ldr Instr         //Return β-normal form if known to not diverge (cached), otherwise return raw
```

TODO:
1) write reference implementation of evaluation of each instruction
2) prove soundness and completeness of each instruction wrt what they are intended to accomplish

## Kinding Instruction Set (CISC)

The RISC instruction set is quite confusing to use directly. Therefore we have additional aliases for common combinations of instructions.

```lsts
... TBD ...
```

## Rank-Level Normalization

The rank-level normalization algorithm can be thought of as a constant-time total function: `Integer -> Integer`.
Rank-level normalization implements the reflective `jmp` instruction above.

This rank-level normalization is the most fundamental basis of the "flexible soundness" guarantees in LSTS.
Basically this compartmentalization permits a more haphazard approach to development and deployment of new type systems.
Creating sound type systems is still desireable, but now there are decidability guiderails during the development of new stuff.

There are many ways to approach rank-level decidability, but it is at most only memorization.
The simplest decidability check will return `Undecided` and never provide any information.
Anything more than that is rote memorization of rule-dependent logic from the kinding system.
