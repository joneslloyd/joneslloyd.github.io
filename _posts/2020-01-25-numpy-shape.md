# NumPy shape

__UPDATE:

I think all my confusion over `numpy.shape` might have been caused by a combination of tiredness and not reading the output array notation correctly.

I saw this:
![Screenshot from lesson one, part two of the Fast AI course](https://joneslloyd.github.io/images/part-2-lesson-1-highlight.png)

...and thought that, in each case, the outer square brackets denoted one array (row), then the next brackets denoted another. I'm pretty sure this is a PHP thing.

Either way, in this context, that's not the case. I was wrong!

~~Coming from a PHP background, I was left really, *really* confused by `numpy.shape` as referenced in lesson 1 of part 2 of the course ([about here](https://youtu.be/4u8FxNEDUeg?t=3433){:target="_blank" rel="noopener"}).

~~Here's why – Hopefully this helps other confused people in the same boat.

~~Jeremy gives some code like this:
`a = tensor([10,20,30]);`

~~and here's its shape:
`torch.Size([3])`

~~...a rank one tensor with three values

~~And we can add a new dimension to it, in the first dimension, like this:
`b = a.unsqueeze(0);`

~~...resulting in this:
`tensor([[10, 20, 30]])`

~~And we can examine the shape of `b` like this:
`b.shape;`

~~...which gives:
`torch.Size([1, 3])`

~~...which is expected, as it has one row (an array) and three columns (the three integers).

~~*HOWEVER*

~~This is what confused me...

~~If we were to instead add a new dimension to `a`, but this time on its *second* dimension (ie, to each integer)...
`c = a.unsqueeze(1);` (resulting in ```tensor([[10],
        [20],
        [30]])```)

~~...the shape would be *this*:
`torch.Size([3, 1])`

~~!??!?!

~~According to [the documentation](https://numpy.org/devdocs/reference/generated/numpy.shape.html){:target="_blank" rel="noopener"} "*The elements of the shape tuple give the lengths of the corresponding array dimensions*"

~~To me, what is returned by the shape property here implies that we have a rank two tensor with three rows, each of which contain one column.

~~But in reality we have one row containing three columns (I think).

~~I did some Googling, and [this](https://stackoverflow.com/a/42465046/2869234){:target="_blank" rel="noopener"} and [this](https://stackoverflow.com/a/47614552/2869234){:target="_blank" rel="noopener"} helped to clear things up for me.

~~[This may also be of use](https://note.nkmk.me/en/python-numpy-ndarray-ndim-shape-size/){:target="_blank" rel="noopener"}.

~~TLDR: The `shape` property returns, to me, inconsistent information depending on the number of the dimensions of the array upon which it is invoked.
