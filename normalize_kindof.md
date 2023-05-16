```lsts
type KindOf = Diverges | Converges(Integer) | Undecided(Integer,Integer,Integer); 

let kind_of(rule: Integer, l: Integer, r: Integer): KindOf = {
   //The kind evaluation rules go here
}
```

The rank level normalization algorithm is OK.
The kind level normalization algorithm is TBD.

This rank level normalization is the basis of the "flexible soundness" guarantees in LSTS.
Basically this compartmentalization permits a more haphazard approach to development and deployment of new type systems.
Creating sound type systems is still desireable, but now there are decidability guiderails during the development of new stuff.
