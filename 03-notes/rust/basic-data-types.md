# Basic Data Types

The difference between signed and unsigned ints, is that an unsigned int is only positive vals and a signed int can be negative.

The two different types take up the same amount of bit-space, so a signed int has a range that is the same, but offset to an unsigned int (offset by half the total range).

This meanst that an unsigned 8bit int has a range of 0-255, and a signed 8bit int has a change of -128-127. Generally we would express these in notation, being:

GENERAL RULE
`-(2^n-1)` to `2^n-1 -1` 

So if you have an eight bit int, that would be negative 2 to the power of seven (128) and two to the power of seven minus 1 (127)

Unsigned bits are easier to maths, because it's just 2^n -1 on the upper end and zero on the lower end. You don't need to worrying about n-1 in the power of section, because you don't need it to be 50% of the val.
