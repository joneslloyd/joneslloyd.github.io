# NumPy shape

Coming from a PHP background, I was left really, *really* confused by `numpy.shape` as referenced in lesson 1 of part 2 of the course ([about here](https://youtu.be/4u8FxNEDUeg?t=3433)).

Here's why! Hopefully this helps other confused people in the same boat.

Jeremy gives some code like this:
`a = tensor([10,20,30]);`

and here's its shape:
`torch.Size([3])`

...a rank one tensor with three values

And we can add a new dimension to it, in the first dimension, like this:
`b = a.unsqueeze(0);`

...resulting in this:
`tensor([[10, 20, 30]])`

And we can examine the shape of `b` like this:
`b.shape;`

...which gives:
`torch.Size([1, 3])`

...which is expected, as it has one row (an array) and three columns (the three integers).

*HOWEVER*

This is what confused me...

If we were to instead add a new dimension to `a`, but this time on its *second* dimension (ie, to each integer)...
`c = a.unsqueeze(1);` (resulting in ```tensor([[10],
        [20],
        [30]])```)

...the shape would be *this*:
`torch.Size([3, 1])`

!??!?!

According to [the documentation](https://numpy.org/devdocs/reference/generated/numpy.shape.html) "*The elements of the shape tuple give the lengths of the corresponding array dimensions*"

To me, what is returned by the shape property here implies that we have a rank two tensor with three rows, each of which contain one column.

Bun in reality we have one row containing three columns (I think).

I did some Googling, and [this](https://stackoverflow.com/a/42465046/2869234) and [this](https://stackoverflow.com/a/47614552/2869234) helped to clear things up for me.

[This may also be of use](https://note.nkmk.me/en/python-numpy-ndarray-ndim-shape-size/).

TLDR: The `shape` property returns, to me, inconsistent information depending on the number of the dimensions of the array upon which it is invoked.
