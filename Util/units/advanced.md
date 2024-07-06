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

### Instantiating Quantities

You may have noticed that throughout the introduction and the advanced section up
until now, we've really avoided actually instantiating quantities with units. The 
reason is that there are two ways, with one of them being exclusive to Kotlin users.

#### Unit Constructors 

This is the way you're forced to do it if you are a Java user, and is a lot less
elegant and natural than the following method.

```kt
val tile = Distance(FEET, 2.0)

val range = Angle(RADIAN, WRAPPING, PI)

val threshold = Current(MILLI_AMP, 5500.0)
```

In Java:
```java
Distance tile = new Distance(FEET, 2.0)

Angle range = new Angle(RADIAN, WRAPPING, PI)

Current threshold = new Current(MILLI_AMP, 5500.0)
```

#### Kotlin Extensions

Kotlin offers a much more elegant approach. By utilizing 
[Kotlin extension functions](https://kotlinlang.org/docs/extensions.html) (also
known as [monkey patching](https://en.wikipedia.org/wiki/Monkey_patch) in other 
languages), we can have suffixes just like we would write units out in real life.

```kt
// different distance units
val tile = 2.feet
val aura = 50.mm

// different wrapping modes
val a = PI.rad
val b = PI.linearRad
val c = PI.wrappedRad
```

> **NOTE:** Extensions are not yet offered on `Current`s due to its placement in
> the `Core` package instead of the `Util`s package as of the most current Dairy
> release, `pre-release-5`. This will be reworked in a future patch.

#### Angle Extensions

### Usage with `ComplexController<T>`

### Defining your own `Unit`s

- angles
- complex controllers
- unit conversion behavior with arithmetic
