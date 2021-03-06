==================
Potato Programming
==================

*One potato, two potato, three potato, four....*

**Potato programming** is the sort of programming that encourages you to write
your own 'for' loops and build up / tear down data structures, rather than
passing vectors or iterators down through an interface in such a way that
would allow a smarter version of that interface to be more efficient.

In other words, this is the potato-programming way to add a file containing
lines of numbers::

    f = file('test.numbers')
    accum = 0.
    for line in f:
        accum += float(line)
    print accum

Contrast this with::

    f = file('test.numbers')
    print sum(map(float, f))

This term was coined by R0ml Lefkowitz.

See also: [PotatoProgrammingExplained Potato Programming Explained]
