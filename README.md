# sdm-julia

Julia implementation of Pentti Kanerva's sparse distributed memory.

The essential content of this repository is an IJulia/Jupyter notebook with code to accompany the article ["The Mind Wanders,"](http://bit-player.org/2018/the-mind-wanders) published in July 2018 on [bit-player.org](http://bit-player.org). The primary reference for all the underlying ideas is the following book: Kanerva, Pentti. 1988. _Sparse Distributed Memory_. Cambridge, Mass.: MIT Press.

The notebook file sdm.ipynb was written for [Julia](https://julialang.org/) version 0.6.3. The file sdmj7.ipynb is updated to run under Julia 0.7 or 1.0.

### IMPLEMENTATION NOTES

Sparse distributed memory is all about computing with high-dimensional binary
vectors—long sequences of 0s and 1s interpreted as the coordinates of the
vertices of a hypercube in a high-dimensional space. In the examples considered
here the length of the vectors is generally 1,000 bits, and so there are $2^{1000}$
possible vectors, or patterns.

The data structure I have chosen for representing these patterns is the Julia `BitVector`, an
array of single bits, packed into 64-bit words. The standard Julia input and
output routines interpret these bits as Boolean values (`false` and `true`), but I
promise it's safe to think of them as 0s and 1s. A big advantage of using
`BitVectors` is that primitive bitwise operations are executed in parallel on
all the bits of a machine word. On a 64-bit processor,
`xor`-ing two 1,000-bit vectors requires only 16 operations instead of 1,000.
(As it happens, `xor` is the operation we're most concerned with.)

This code was written with the aim of exploring the ideas behind sparse distributed memory. It does not create a practical system for *using* sparse distributed memory. (I'm not sure such a system can exist without specialized hardware.)
