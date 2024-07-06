## advanced

### Distance Units

### Positional Units

### Angle Units
`Angle`s differ in that there is special behavior to be considered when performing
arithmetic. As angles are defined in terms of rotation about a point, there exists
the possibility of "wrap-around", or having an angle with a magnitude greater than
that of a full revolution.

There are two `AngleUnits` implemented in Dairy: `RADIAN`, and `DEGREE`.

Additionally, to handle wrap-around consistently, there is an additional `Wrapping`
behavior enum that is to be passed; its members are `WRAPPING` and `LINEAR`.

In terms of the expression `α + β`, where `α, β : Angle`
  - **`WRAPPING`:** becomes smallest positive coterminal angle to `α + β`
    - `225° + 180° -> 405° -> 45°`
    - `10° + 360° -> 370° -> 10°`
  - **`LINEAR`:** does not reset on a rotation, adds the magnitudes of `α` and `β`
    - `225° + 180° -> 405°`
    - `10° + 360° -> 370°`

The wrap-around value is defined as `2π` and `360°` for `RADIAN` and `DEGREE`
respectively.

### Arithmetic Behavior

The entire unit system is completely non-mutating; nothing is modified, only 
created. All quantities have both a unit and magnitude, and so it is natural to
ask what the actual unit of any given expression becomes. 

For all defined units, the right-hand side of an operation is converted to match
the units of the left-hand side. Consequently, it is important to note that this
means that commutativity does not hold for both magnitudes **and** units.

For example, take the case of an addition between two currents.
```kt
val current = Current(1, AMP)

val a = current.intoMilliAmps() + current.intoAmps()
val b = current.intoAmps() + current.intoMilliAmps()

a.value // 2000
a.unit // MILLI_AMP

b.value // 2
b.unit // AMP
```

By changing the order of the operands, although the actual value of the sums are
the same, their value is different as the units that express them are different.

### Angle Arithmetic Wrapping Behavior

Angles also have their own unique behavior within arithmetic expressions. Recall
the wrapping behavior of `Angle`. What happens when we add two angles that have
differing wrapping behaviors?

For the purposes of all defined arithmetic methods, `LINEAR` takes strict priority.
If the `wrapping` of either operand of an operation is `LINEAR`, the result will 
have a wrapping behavior of `LINEAR`. If all operands are of wrapping behavior
`WRAPPING`, then the result has a wrapping behavior of `WRAPPING`.

### Kotlin Extensions

#### Angle Extensions

### Usage with `ComplexController<T>`

### Defining your own `Unit`s

- angles
- complex controllers
- unit conversion behavior with arithmetic
