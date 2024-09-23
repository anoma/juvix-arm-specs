# Feedback on juvix-arm-specs

1. Anoma/Transaction/Action.juvix#L53-L62

https://github.com/anoma/juvix-arm-specs/blob/71c673348175ab76f5368802e2a38bb89357a4ce/Anoma/Transaction/Action.juvix#L53-L62

We should make it clear that the `ProofRecord` type is parametrized by `Proof`, `VerifyingKey` and `ProvingKey` and `Witness`. This is not possible to express in a Juvix container type (e.g `Set`) because all values in a container must have the same type and we don't have existential types.

There is a different `ProofRecord` for each proving system supported by the spec. Proofs could be ZK proofs, proof of membership of the commitment accumulator, etc.

2. Anoma/Proving/System.juvix#L11-L16

https://github.com/anoma/juvix-arm-specs/blob/71c673348175ab76f5368802e2a38bb89357a4ce/Anoma/Proving/System.juvix#L11-L16

I don't follow the definition of `ProvingSystem` but is likely my misunderstaning. Why do the `Instance` and the `Witness` depend on the `Resource`? Action validity will depend on several `ProvingSystem`s that will each have their own `Instance` and `Witness` types (e.g a proof of membership of a commitment accumulator vs a proof associated to an application).

Perhaps this is an option to define the proving system:

```
--- A record describing a proof record, a map entry constituted by a ;VerifyingKey; as the lookup-key
--- and a ;Proof; and associated ;Resource.Witness; as the value.
ProofRecord (Proof VerifyingKey Witness : Type) : Type := Pair VerifyingKey (Pair Proof Witness);

--- A set containing ;ProofRecord;s
axiom ProofRecordSet :
  (Proof : Type)
  -> (VerifyingKey : Type)
  -> (ProvingKey : Type)
  -> (Instance : Type)
  -> (Witness : Type) -> Type;

trait
type ProvingSystem Proof VerifyingKey ProvingKey Instance Witness :=
  mkProvingSystem {
    --- Creates a ;Proof; given a ;ProvingKey;, public inputs (;Instance;), and
    --- private inputs (;Witness;).
    prove : (provingKey : ProvingKey)
      -> (publicInputs : Instance)
      -> (privateInputs : Witness)
      -> Proof;

    --- Verfies a ;ProofRecord;.
    verify : (proofRecord : ProofRecord Proof VerifyingKey Witness) -> Bool
  };

```
