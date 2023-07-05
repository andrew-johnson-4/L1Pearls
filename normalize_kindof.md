## Kinding Algorithm

The kinding algorithm is designed to be expressive, yet strongly-normalizing and sound.
The kind system is extensible, being defined from within the AST.
Proper *kinds* are actually just types-for-types that determine what rules apply to which contexts.
Through this process we implement a rich *type* system, built from a simple *kind* system.

The kind system runs in a constant-space abstract computer.
Programs here are constant-space, but not constant-time.
The abstract computer is designed to traverse the AST to get whatever it needs.
The goal of running a program in the kind system is to gain information at the type level.

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
           | Instr !           //termsof x: return all terms bound in global context having the type x
           | Instr ?           //typesof x: return all types bound in global context having the term x
           | Instr $$$ Instr   //concat x y. x y
           | label Instr       //TODO: store information in global context
           | jmp Instr         //eval-hard x: return β-normal form, may diverge
           | $$$!              //TODO: normalize global context terms
           | $$$?              //TODO: normalize global context types
           | $$Instr           //ctx x: return all variables v in x, where v is not bound as a type in global context
           | $(Instr)          //free x: return all variables v in x, where v is not bound as a term in global context
           | mov Instr Instr   //infer x y: localized type inference of term x in context y
           | ldr Instr         //eval-soft x: return β-normal form if known to not diverge (cached), otherwise return raw
```

TODO:
1) write reference implementation of evaluation of each instruction
2) prove the total reduction of these rules into CoC (actual reduction might be infeasible)

## Kinding Instruction Set (CISC)

The RISC instruction set is quite confusing to use directly. Therefore we have additional aliases for common combinations of instructions.

```lsts
noop             //Assuming that the result is discarded, this operator does nothing other than consume processing time

... more TBD ...
```
