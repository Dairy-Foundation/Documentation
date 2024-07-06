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

#### Angle Wrapping Behavior

### Kotlin Extensions

#### Angle Extensions

### Usage with `ComplexController<T>`

### Defining your own `Unit`s

- angles
- complex controllers
- unit conversion behavior with arithmetic
