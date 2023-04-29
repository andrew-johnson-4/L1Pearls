# Notation

🟧 Monomorphic (Sequential Process)

🟨 Polymorphic (Merging of Parallel Process)

🟦 Discriminator (Decision Process)

🟪 PolyFeedback (Signal to Parallel Process)

🟩 MonoFeedback (Signal to Sequential Process)

🟥 Interaction Net (Parallel Process)


# A nontrivial parallel runtime

🟧 🟦 🟩
 
🟨 🟪 🟥 

This runtime features a single leader process and many follower processes.
The interaction net is a great model of parallel rewriting computation.
Unfortunately interaction nets can have trouble modelling side effects of input and output.
That is where the leader process comes in to create a pseudo-sequential interface for an otherwise parallel process.
