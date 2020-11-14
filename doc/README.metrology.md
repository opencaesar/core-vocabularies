# Metrology Vocabulary

The metrology vocabulary is based on Prof. Rene Dybkaer's seminal work, [An ontology on property](http://ontology.iupac.org/), that influenced the work of the 3rd revision of the International Vocabulary of Metrology, VIM, see: https://www.bipm.org/en/publications/guides/rationale-vim3.html

The following describes the Patterns involved in applying the metrology vocabulary for modeling a subset of 
the [ISO/IEC 80000 collection of metrology standards](https://en.wikipedia.org/wiki/ISO/IEC_80000).
The same patterns should be applicable for modeling project-specific quantities and units as well as off-system quantities and units such as 
the [English Engineering Units](https://en.wikipedia.org/wiki/English_Engineering_Units).

## Organizing a vocabulary of quantities and units

<details>
<summary>Details</summary>

Organizing a vocabulary of quantities and units involves several OML files as described below:

1) Defining an OML vocabulary of units.

   Note that typically, a vocabulary of quantities depends on other vocabularies of quantities.
   For example, ISO 80000-4 depends on ISO 80000-3. This dependence is reflected in OML as extensions.

   Example: [src/oml/iso.org/80000-4-units.oml](../src/oml/iso.org/80000-4-units.oml)

   For guidance, see Steps 1-4 in [Defining units](#defining-units).

2) Defining an OML vocabulary of quantities extending the vocabulary of units.

   Note that typically, a vocabulary of quantities depends on other vocabularies of quantities.
   For example, ISO 80000-4 depends on ISO 80000-3. This dependence is reflected in OML as extensions.

   Example: [src/oml/iso.org/80000-4-quantities.oml](../src/oml/iso.org/80000-4-quantities.oml)

   For guidance, see Step 1 in [Defining quantities](#defining-quantities).

3) Defining an OML vocabulary of quantity derivation consistency rules extending the vocabulary of quantities.

   If the vocabulary of quantities depends on another vocabulary of quantities,
   then there should be an extension to the other vocabulary
   of quantity derivation consistency rules.

   Example: [src/oml/iso.org/80000-4-derivation-consistency.oml](../src/oml/iso.org/80000-4-derivation-consistency.oml)

   For guidance, see Step 2 in [Defining quantities](#defining-quantities).

4) Defining an OML description for instances of the quantities, units and additions to the international systems of quantities and units, as applicable.
   
   Example: [src/oml/iso.org/80000-4-instances.oml](../src/oml/iso.org/80000-4-instances.oml)

   For guidance, see Step 3 in [Defining quantities](#defining-quantities) and Step 5 in [Defining units](#defining-units).

5) Defining an OML vocabulary of quantity values extending the vocabulary of units and the description of instances.

   Example: [src/oml/iso.org/80000-4-quantityValues.oml](../src/oml/iso.org/80000-4-quantityValues.oml)

   For guidance, see Step 6 [Defining units](#defining-units).

</details>

## Defining quantities

Note that the difference between kind-of-quantity and quantity is that a kind-of-quantity is a class-level definition in a vocabulary independent of any physical object whereas a quantity is a particular instance of a kind-of-quantity associated to a particular physical object.

Each of part 3 through 14 of the ISO 80000 collection define kinds of quantities and corresponding measurement units. The following describes the process involved in defining a quantity.

Defining a quantity involves the following:
1) Determine whether the quantity is base or derived.
   * Define a base quantity as a specialization of `metrology:IndependentUnitaryQuantity`
   * Define a derived quantity as a specialization of `metrology:DerivedUnitaryQuantity` with 
  specializations of `metrology:HasFactorForUnitaryQuantity` for each quantity it depends on.
2) For a derived quantity, define a derivation consistency rule.
3) Define an instance of the quantity.

<details>
<summary>Details</summary>

Determine whether the quantity is a base or derived quantity in
the ISO 80000 system of quantities. See [ISO-80000-1:2009 section 3.7](https://www.iso.org/obp/ui/#iso:std:iso:80000:-1:ed-1:v1:en) 

### Step 1: Defining a base quantity

<details>
<summary>Details</summary>

Note that, by definition, every base quantity has a dimension different than 1;
this is conveyed in the metrology vocabulary with the boolean property `metrology:isDimensionlessQuantity`.

Use the following as an example from [src/oml/iso.org/80000-3-quantities.oml](../src/oml/iso.org/80000-3-quantities.oml).

Given the following definition from [ISO 80000-3:2019](https://www.iso.org/obp/ui/#iso:std:iso:80000:-3:ed-2:v1:en):

![](80000-3-length-metre.png)

Here is the corresponding definition using the metrology vocabulary:

```oml
@dc:^source "https://www.iso.org/obp/ui/#iso:std:iso:80000:-3:ed-2:v1:en"
vocabulary <http://iso.org/80000-3-quantities> with # as 80000-3-quantities {
    extends <http://iso.org/80000-3-units>

    @dc:^source "3-1.1"
    concept ^length :> metrology:IndependentUnitaryQuantity [
        restricts scalar property metrology:isDimensionlessQuantity to false^^xsd:boolean
        restricts all relation metrology:hasMagnitude to 80000-3-units:UnitOfLengthValue 
    ]

    ...
}
```

The last restriction above says that the `metrology:hasMagnitude` of every `80000-3-quantities:length` must
be a `metrology:QuantityValue` whose `metrology:hasReference` must be a `metrology:MeasurementUnit` that is a `80000-3-units:UnitOfLengthValue`.

</details>

### Step 1: Defining a derived quantity

<details>
<summary>Details</summary>

A derived quantity is defined as the product of other quantities raised to a rational exponent.
This is a dimensional relationship that enables calculating the effective dimension of every quantity
in a system of quantities. Note that this dimensional relationship between a derived quantity
and its factored quantities can be very different than the mathematical relationship for 
the same derived quantity as explained in the notion of quantity dimension 
in [ISO 80000-1:2009, 3.7](https://www.iso.org/obp/ui/#iso:std:iso:80000:-1:ed-1:v1:en).


Note when defining a derived quantity with `metrology:isDimensionlessQuantity=true`,
it is important to retain the non-simplified product factors in order to preserve 
the metrological provenance of this derived quantity with respect to other quantities.
Without this provenance, all dimensionless quantities would be equivalent to each other,
which is absurd in metrology. Consider for example, efficiency in mechanics (ISO 80000-4:2019, 4.29) and
relative humidity in thermodynamics (ISO 80000-5:2019, 5.33) are both dimensionless quantities but they 
are clearly not equivalent to each other.

Use the following as an example from [src/oml/iso.org/80000-4-quantities.oml](../src/oml/iso.org/80000-4-quantities.oml).

Given the following definition from [ISO 80000-4:2019](https://www.iso.org/obp/ui/#iso:std:iso:80000:-4:ed-2:v1:en):

![](80000-4-impulse-newton-second.png)

Here is the corresponding definition using the metrology vocabulary.
Notice the difference between the mathematical equation of impulse and 
the corresponding quantity dimension formula as a product of quantities raised to a rational exponent.
It is unfortunate that the ISO 80000 standards lack clear guidance about converting
mathematical formulas into corresponding dimension formulas.

```oml
@dc:^source "https://www.iso.org/obp/ui/#iso:std:iso:80000:-4:ed-2:v1:en"
vocabulary <http://iso.org/80000-4-quantities> with # as 80000-4-quantities {
    extends <http://iso.org/80000-4-units>
    extends <http://iso.org/80000-3-quantities>
  
    ...

    @dc:^source "4-10"
    concept impulse :> metrology:DerivedUnitaryQuantity [
        restricts scalar property metrology:isDimensionlessQuantity to false^^xsd:boolean
        restricts all relation  metrology:hasMagnitude to 80000-4-units:UnitOfForceValue
    ]
    
    relation entity impulse-of-force :> metrology:HasFactorForUnitaryQuantity [
        from impulse
        to force
        forward impulse-of-force-forward
        functional
        restricts scalar property metrology:exponent to "1/1"^^owl:rational
    ]

    relation entity impulse-of-duration :> metrology:HasFactorForUnitaryQuantity [
        from impulse
        to 80000-3-quantities:duration
        forward impulse-of-duration-forward
        functional
        restricts scalar property metrology:exponent to "1/1"^^owl:rational
    ]

    ...
}
```

</details>

### Step 2: Defining a derived quantity consistency rule

A derivation consistency rule facilitates detecting incorrect usage of 
derived quantities with respect to its dependency on other quantities.

For example, deriving the velocity of a vehicle based on the position-vector of something else.

Use the following as an example from [src/oml/iso.org/80000-3-derivation-consistency.oml](../src/oml/iso.org/80000-3-derivation-consistency.oml).

```oml
@dc:^description "Optional consistency rules to enforce that derived quantities are about the same object as the objects of their quantity factors."
vocabulary <http://iso.org/80000-3-derivation-consistency> with # as 80000-3-derivation-consistency {
  extends <http://iso.org/80000-3-quantities>

  ...

  @dc:^description "
  If a velocity, x, is derived from a position-vector, y, and a duration, z,
  then x, y, and z must be quantities of the same object."
  rule velocity-derivation-consistency [
    // x is the velocity quantity of an object, xo.
    metrology:Object(xo) ^
    metrology:Quantity(x) ^
    metrology:isQuantityOf(x,xo) ^
    80000-3-quantities:velocity(x) ^

    // y is the position-vector quantity of an object, yo.
    metrology:Object(yo) ^
    metrology:Quantity(y) ^
    metrology:isQuantityOf(y,yo) ^
    80000-3-quantities:position-vector(y) ^

    // z is the duration quantity of an object, zo.
    metrology:Object(zo) ^
    metrology:Quantity(z) ^
    metrology:isQuantityOf(z,zo) ^
    80000-3-quantities:duration(z) ^

    // if there is a dimensional calculus constraint relating x as a derived from y and z
    80000-3-quantities:velocity-of-position-vector-forward(x,y) ^
    80000-3-quantities:velocity-of-duration-forward(x,z)
  
    ->

    // then all the quantities must be of the same object.
    sameAs(xo,yo) ^ sameAs(xo,zo)
  ]

  ...
}
```

In practice, it is helpful to use SPARQL rules to find consistent and inconsistent instances of
derived quantities. For example, see [src/sparql/velocity-consistent-derivation.sparql](src/sparql/velocity-consistent-derivation.sparql) and [src/sparql/velocity-inconsistent-derivation.sparql](src/sparql/velocity-inconsistent-derivation.sparql).

### Step 3: Defining instances

Note that such instances are kind-of-quantities in the sense of Prof. Dybkaer's ontology
because they are unrelated to any `metrology:Object`.

Where applicable, it is important to record whether a quantity is included in the International System of Quantities.

<details>
<summary>Details</summary>

Use the following as an example from [src/oml/iso.org/80000-3-instances.oml](../src/oml/iso.org/80000-3-instances.oml).

```oml
description <http://iso.org/80000-3-instances> with # as 80000-3-instances {
  uses <http://iso.org/80000-3-quantities>

  @dc:^source "3-1"
  ci ^length : 80000-3-quantities:^length

  ri systemOfQuantities-length : metrology:SystemHasUnitaryQuantity [
    from 80000-instances:systemOfQuantities
    to ^length
    metrology:isBaseQuantity "true"^^xsd:boolean
  ]

  ...

  @dc:^source "3-10"
  ci velocity : 80000-3-quantities:velocity

  ri systemOfQuantities-velocity : metrology:SystemHasUnitaryQuantity [
    from 80000-instances:systemOfQuantities
    to velocity
    metrology:isBaseQuantity "false"^^xsd:boolean
  ]

  ...
}
```

</details>

</details>

## Defining units

Each of part 3 through 14 of the ISO 80000 collection define kinds of quantities and corresponding measurement units. The following describes the process involved in defining a measurement unit.

Defining a unit involves the following:
1) Define a specialization of `metrology:UnitaryQuantityValue` for representing quantity values whose reference is that unit.
2) Define a specialization of `metrology:CoherentMeasurementUnit` as the root taxonomy for the unit including its prefix variations.
3) Determine whether the unit is base or derived.
   * Define a base unit as a specialization of `metrology:IndependentMeasurementUnit`
   * Define a derived unit as a specialization of `metrology:DerivedMeasurementUnit` with 
  specializations of `metrology:HasFactorForMeasurementUnit` for each unit it depends on.
4) Define specializations of the unit for the relevant prefixes
5) Define instances of the unit and of the prefixed units
6) Define a specialization of `80000-concepts:UnitaryQuantityMagnitude` to facilitate modeling a quantity value and the magnitude relationship.

<details>
<summary>Details</summary>

### Steps 1-4: Defining a base unit

<details>
<summary>Details</summary>

Use the following as an example from [src/oml/iso.org/80000-3-units.oml](../src/oml/iso.org/80000-3-units.oml).

Given the following definition from [ISO 80000-3:2019](https://www.iso.org/obp/ui/#iso:std:iso:80000:-3:ed-2:v1:en):

![](80000-3-length-metre.png)

Here is the corresponding definition using the metrology vocabulary:

```oml
@dc:^source "https://www.iso.org/obp/ui/#iso:std:iso:80000:-3:ed-2:v1:en"
vocabulary <http://iso.org/80000-3-units> with # as 80000-3-units {
    extends <http://iso.org/80000-concepts>
    uses <http://iso.org/80000-1>

    -- step 1
    aspect UnitOfLengthValue :> metrology:UnitaryQuantityValue [
        restricts all relation metrology:hasReference to UnitOfLength
    ]
    
    -- step 2
    @dc:^source "3-1"
    concept UnitOfLength :> metrology:CoherentMeasurementUnit [
        restricts scalar property metrology:isDimensionlessMeasurementUnit to false^^xsd:boolean
    ]

    -- step 3
    concept metre :> UnitOfLength, metrology:IndependentMeasurementUnit, 80000-concepts:MeasurementUnit

    -- step 4
    concept kilometre :> UnitOfLength, metrology:PrefixedMeasurementUnit, 80000-concepts:MeasurementUnit [
        restricts relation metrology:hasPrefix to 80000-1:kilo
    ]
}
```

</details>

### Steps 1-4: Defining a derived unit

<details>
<summary>Details</summary>

Use the following as an example from [src/oml/iso.org/80000-4-units.oml](../src/oml/iso.org/80000-4-units.oml).


Given the following definition from [ISO 80000-4:2019](https://www.iso.org/obp/ui/#iso:std:iso:80000:-4:ed-2:v1:en):

![](80000-4-impulse-newton-second.png)

Here is the corresponding definition using the metrology vocabulary:

```oml
@dc:^source "https://www.iso.org/obp/ui/#iso:std:iso:80000:-4:ed-2:v1:en"
vocabulary <http://iso.org/80000-4-units> with # as 80000-4-units {
    extends <http://iso.org/80000-3-units>
    uses <http://iso.org/80000-1>
  
    ...

    -- step 1
    aspect UnitOfImpulseValue :> metrology:UnitaryQuantityValue [
        restricts all relation metrology:hasReference to UnitOfImpulse
    ]

    -- step 2
    @dc:^source "4-10"
    concept UnitOfImpulse :> metrology:CoherentMeasurementUnit [
        restricts scalar property metrology:isDimensionlessMeasurementUnit to false^^xsd:boolean
    ]

    -- step 3
    concept newton-second :> UnitOfImpulse, metrology:DerivedMeasurementUnit, 80000-concepts:MeasurementUnit 

    relation entity newton-second-of-newton :> metrology:HasFactorForMeasurementUnit [
        from newton-second
        to newton
        forward newton-second-of-newton-factor
        restricts scalar property metrology:exponent to "1/1"^^owl:rational
    ]

    relation entity newton-second-of-second :> metrology:HasFactorForMeasurementUnit [
        from newton-second
        to 80000-3-units:metre-per-second
        forward newton-second-of-second-factor
        restricts scalar property metrology:exponent to "1/1"^^owl:rational
    ]

    -- step 4
    concept newton-millisecond :> UnitOfImpulse, metrology:DerivedMeasurementUnit, 80000-concepts:MeasurementUnit 

    relation entity newton-millisecond-of-newton :> metrology:HasFactorForMeasurementUnit [
        from newton-millisecond
        to newton
        forward newton-millisecond-of-newton-factor
        restricts scalar property metrology:exponent to "1/1"^^owl:rational
    ]

    relation entity newton-millisecond-of-millisecond :> metrology:HasFactorForMeasurementUnit [
        from newton-millisecond
        to 80000-3-units:millisecond
        forward newton-millisecond-of-millisecond-factor
        restricts scalar property metrology:exponent to "1/1"^^owl:rational
    ]

    ...
}
```

</details>

### Step 5: Defining instances

<details>
<summary>Details</summary>

Use the following as an example from [src/oml/iso.org/80000-3-instances.oml](../src/oml/iso.org/80000-3-instances.oml).


```oml
description <http://iso.org/80000-3-instances> with # as 80000-3-instances {
  uses <http://iso.org/80000-3-quantities>

  @dc:^source "3-1"
  ci metre : 80000-3-units:metre
 
  ci kilometre : 80000-3-units:kilometre


  @dc:^source "3-10"
  ci metre-per-second : 80000-3-units:metre-per-second

  ci kilometre-per-second : 80000-3-units:kilometre-per-second
```

</details>

### Step 6: Defining convenience quantity magnitudes

<details>
<summary>Details</summary>

Use the following as an example from [src/oml/iso.org/80000-3-quantityValues.oml](../src/oml/iso.org/80000-3-quantityValues.oml).

```oml
@dc:^description "This vocabulary provides convenience specializations of metrology:UnitaryQuantityValue
as concepts for every metrology:MeasurementUnit defined in http://iso.org/80000-3-units.

Note that this vocabulary reflects an opinionated usage of metrology:UnitaryQuantityValue
as an OML concept and does not exclude in any way other possible usages."
vocabulary <http://iso.org/80000-3-quantityValues> with # as 80000-3-quantityValues {
  extends <http://iso.org/80000-concepts>
  extends <http://iso.org/80000-3-units>
  uses <http://iso.org/80000-3-instances>

  relation entity metre-magnitude :> 80000-concepts:UnitaryQuantityMagnitude, 80000-3-units:UnitOfLengthValue [
     from metrology:UnitaryQuantity
     to metre-magnitude
     forward hasMetreMagnitude
     restricts relation metrology:hasReference to 80000-3-instances:metre
  ]

  relation entity kilometre-magnitude :> 80000-concepts:UnitaryQuantityMagnitude, 80000-3-units:UnitOfLengthValue [
     from metrology:UnitaryQuantity
     to kilometre-magnitude
     forward hasKilometreMagnitude
     restricts relation metrology:hasReference to 80000-3-instances:kilometre
  ]

  ...

    relation entity metre-per-second-magnitude :> 80000-concepts:UnitaryQuantityMagnitude, 80000-3-units:UnitOfVelocityValue [
     from metrology:UnitaryQuantity
     to metre-per-second-magnitude
     forward hasMetrePer-SecondMagnitude
     restricts relation metrology:hasReference to 80000-3-instances:metre-per-second
  ]

  relation entity kilometre-per-second-magnitude :> 80000-concepts:UnitaryQuantityMagnitude, 80000-3-units:UnitOfVelocityValue [
     from metrology:UnitaryQuantity
     to kilometre-per-second-magnitude
     forward hasKilometrePerSecondMagnitude
     restricts relation metrology:hasReference to 80000-3-instances:kilometre-per-second
  ]

  ...
}
```

</details>

</details>

## Defining prefixes

In the collection of ISO 80000 metrology standards, there are only two kinds of prefixes: decimal and binary.
In the metrology vocabulary, note that the actual value of a prefix is not explicitly represented in OML.
However, each prefix is defined as an instance that could be linked to an external representation in a mathematical tool.

<details>
<summary>Details</summary>

Use the following as an example from [src/oml/iso.org/80000-1-decimalPrefix.oml](../src/oml/iso.org/80000-1-decimalPrefix.oml):

```oml
vocabulary <http://iso.org/80000-1-decimalPrefix> with # as 80000-1-decimalPrefix {
    extends <http://iupac.org/metrology>

    concept DecimalPrefix :> metrology:Prefix
}
```

and from [src/oml/iso.org/80000-1.oml](../src/oml/iso.org/80000-1.oml):

```oml
description <http://iso.org/80000-1> with # as 80000-1 {
    uses <http://iso.org/80000-1-decimalPrefix>

    @dc:^description "10^24"
    ci yotta : 80000-1-decimalPrefix:DecimalPrefix

    ...

    @dc:^description "10^3"
    ci kilo : 80000-1-decimalPrefix:DecimalPrefix

    @dc:^description "10^2"
    ci hecto : 80000-1-decimalPrefix:DecimalPrefix

    @dc:^description "10^1"
    ci deca : 80000-1-decimalPrefix:DecimalPrefix

    @dc:^description "10^-1"
    ci deci : 80000-1-decimalPrefix:DecimalPrefix

    ...

    @dc:^description "10^-24"
    ci yocto : 80000-1-decimalPrefix:DecimalPrefix

}
```

</details>

</details>