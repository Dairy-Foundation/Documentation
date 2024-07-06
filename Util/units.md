# Units

## introduction

Programming is a form of expressing thought. As with any other human endeavour,
we often make mistakes in translating our ideas over to reality. Over the years,
many tools have been created to ensure that what we have written matches what we
would like. In the sciences, we use dimensional analysis and friends to ensure 
that the quantity we ended up with matches against the known shape of what we 
want to find. In programming, we've developed type systems to eliminate a slew
of logical errors.

Dairy borrows a small concept from the sciences with its unit system. There are
three types of units offered throughout Dairy: `Distance`,`Angle`, and `Current`.

There are three "guiding positives" for the units:
1. The user is safe from accidentally using another unit (say, centimeters) 
   when a specific function expected something else (perhaps inches).
2. The user can quickly convert from and to other units when needing varying
   levels of precision.
3. The user can easily define their own units of the same dimension for 
   their specific use-case.

But where and how are they actually used?

### Usage

For the purposes of just general usage of units, we will be playing around
with `Current`s, specifically from `CalcifiedMotor`s.

First, let's get something to play with.
```kt
val motor = Calcified.controlHub.getMotor(0)
```

Now we can access the `current` of the motor:
```kt
val current: Current = motor.current
```

We can get the actual value of the current as so:
```kt
current.value // : Double
```

And if we want to get the unit that was provided:
```kt
// the actual unit of the value given
current.unit // : CurrentUnit
```

We can convert between the units in two different ways:
```kt
// getting the current in a specific unit
current.into(CurrentUnits.AMP).value // : Double

// OR

// quick conversions for "batteries included" units
current.intoAmps()
current.intoMilliAmps()
```

We can also add a bunch of currents together:
```kt
val totalDraw = motors
    .map { it.current }
    .map { it.intoAmps() }
    .fold { a, b -> a + b }
    .value
```

Or maybe we want to compare between them:
```kt
val threshold = Current(AMPS, 5.0)

if (motor.current > threshold) { /* ... */ } 
```

For the purposes of the basic usage and exhibition, we will end here. Dairy
offers helpful utilities when working with `Angle`s as will be shown in the
next section, as well as strong incorporation with the rest of the library.

## advanced
- angles
- complex controllers
- unit conversion behavior with arithmetic

## contributors
- how units are defined
- why things are the way they are