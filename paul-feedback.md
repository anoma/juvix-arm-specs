# Feedback on juvix-arm-specs


1. Anoma/Resource/Types.juvix#L44-L47

https://github.com/anoma/juvix-arm-specs/blob/71c673348175ab76f5368802e2a38bb89357a4ce/Anoma/Resource/Types.juvix#L44-L47

```
--- Provides a function calculating a fixed-size ;RefType; from a dynamically sized ;DataType;.
--- For the private testnet, the requirement can be relaxed by allowing ;RefType; to be dynamically-sized.
trait
type Ref DataType RefType := mkRef {ref : DataType -> RefType};
```

I think you mean to define a function that for each `DataType` returns the corresponding `RefType`. The trait does not do this, it returns a value of a `RefType` for each value of a `DataType`.

It's not possible to do this in Juvix, you need something called an 'open type family' or a trait which can be type-valued:

```
trait RefType A := mkRefType { refType : Type };
```

Perhaps we can remove this and just document the requirement or replace it with another axiom:

```
axiom RefType : Type -> Type
```
