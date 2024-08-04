# Cells

## Cells + RefCell

Cells are Dairy's shared interior mutability reference wrapper.
This sounds complicated, but its not.
Cells basically allow you to wrap some data with a common access interface.

The most basic cell you could use is the `RefCell`. We'll use [Units](../units)
as our inner contents.
```kt
val millimeters = RefCell(10.mm) // this RefCell contains a distance
```

The contents of a Cell are often referred to as the 'contents' or the 'ref'
(short for reference).
We can access the contents like so:
```kt
val ten = millimeters.get()
```
and we can set the contents like so:

```kt
millimeters.accept(12.mm)
```

This is because all Cells implement Consumer and Supplier.
This allows them to be used directly in many places across Dairy, as these two
interfaces are used often.

Additionally, Cells keep track of when access occurred.

```kt
millimeters.timeSincelastGet
millimeters.timeSincelastSet
millimeters.timeSinceLastAccess
```

Its also worth noting that Cells can be used for delegation in Kotlin:
```kt
val distance by millimeters
```
This means that `distance` is of type `Distance`, and when you get, or set it,
the behaviour is delegated to the millimeters RefCell.

RefCell is useful, but Cells can be extended to have a myriad of other
behaviours, we'll go over some of the more common and useful

## LateInitCell

LateInitCell starts with null inside, but insists that it doesn't contain a null
value! It will throw an exception if you try to get the contents and they are
null.

The next cells are all built off the LateInitCell back, so they all share the
following additional utility methods:

```kt
val late = LateInitCell<Distance>()

late.accept(10.mm)
late.initialised() // true
// late.accept(null) this doesn't work
late.invalidate() // now the cell is back to containing null
late.initialised() // false
late.safeGet() // will not throw an error if the contents are null
late.safeInvoke { // only runs if the contents aren't null
    it + 12.mm
} // null

```

## LazyCell

LazyCell will lazily evaluate its contents from a function whenever you attempt
to obtain the contents and they are null.

Combine this with `invalidate()` in order to re-evaluate the contents using the
function.

Several modules have their own wrapper of this, e.g. Core has the
`OpModeLazyCell` and `OpModeFreshLazyCell` and Mercurial has the
`SubsytemObjectCell`. These wrappers provide additional functionality tied to
their specific domains.

```kt
var counter = 0;
val lazy = LazyCell {
    counter++.mm
}

lazy.get() // 0, now counter is 1
lazy.get() // still 0
lazy.invalidate()
lazy.get() // 1, now counter is 2
lazy.get() // still 1
```

## InvalidatingCell

Invalidating Cell is like lazy cell, but has a built in self invalidation
function as well.

```kt
val invalidating = InvalidatingCell({
    counter++.mm
}, { self, contents ->
    contents < 10.mm || self.timeSincelastSet > 1
})
```

Whenever the contents of the cell are accessed, the invalidating function is
checked, to see if the cell should be re-evaulated.

The cell contents should not be accessed through the `self` parameter, as this
will cause stack overflow, this is what the `contents` parameter is for.

### StaleAccessCell

A time-based wrapper for InvalidatingCell.

```kt
val staleAccess = StaleAccessCell(5.1) {
	counter++.mm
}
```

This example will invalidate when it has been 5.1 seconds since last access.

### StaleEvalCell

A time-based wrapper for InvalidatingCell

```kt
val staleEval = StaleEvalCell(5.1) {
	counter++.mm
}
```

This example will invalidate when it has been 5.1 seconds since last eval.

## Further usage

Additional Cells exist, but are likely not so useful to the more common user.
It is also possible to implement your own cells.

If using Core or beyond, you should look at the Cells defined in those modules.
