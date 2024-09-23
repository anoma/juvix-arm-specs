# Feedback on juvix-arm-specs

1. Anoma/Transaction/Action.juvix#L53-L62

https://github.com/anoma/juvix-arm-specs/blob/71c673348175ab76f5368802e2a38bb89357a4ce/Anoma/Transaction/Action.juvix#L53-L62

We should make it clear that the `ProofRecord` type is parametrized by `Proof`, `VerifyingKey` and `ProvingKey` and `Witness`. This is not possible to express in a Juvix container type (e.g `Set`) because all values in a container must have the same type and we don't have existential types.

There is a different `ProofRecord` for each proving system supported by the spec. Proofs could be ZK proofs, proof of membership of the commitment accumulator, etc.

2. Anoma/Proving/System.juvix#L11-L16

https://github.com/anoma/juvix-arm-specs/blob/71c673348175ab76f5368802e2a38bb89357a4ce/Anoma/Proving/System.juvix#L11-L16

I don't follow the definition of `ProvingSystem` but is likely my misunderstaning. Why do the `Instance` and the `Witness` depend on the `Resource`? Action validity will depend on several `ProvingSystem`s that will each have their own `Instance` and `Witness` types (e.g a proof of membership of a commitment accumulator vs a proof associated to an application).

