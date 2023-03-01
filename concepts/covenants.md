# Covenants

In Mel's [data model](data-model.md), every coin/UTXO is locked by a _covenant_, or a program that constrains what sort of transaction can spend it. Common covenants include:

* Checking that a transaction is signed by a particular public key
* Checking that a transaction is signed by some subset of keys (a _multisig_ covenant)
* Encoding arbitrary stateful logic using _self-propagating_ covenants

On the blockchain level, covenants are written in the low-level [MelVM](../resources/melvm-spec.md) language, but in practice covenants are programmed using the high-level [Melodeon](https://melodeonlang.org/) language. More info on specific programming patterns can be found in the [Melodeon guide](https://guide.melodeonlang.org/).

