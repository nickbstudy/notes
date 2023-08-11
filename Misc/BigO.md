Big O notation can objectively describe the efficiency of code without the use of concrete units.  It focuses on how the time and space requirements scale, and generally calculates for the worst case scenario.

Lines that only run once (constants) can be ignored as they don't affect the value.

Perfect display here:

https://www.youtube.com/watch?v=HfIH3czXc-8

Basically ignore any multipliers or additives, O(n) is only the highest exponent.  Simplify anything to the highest exponent. O(10n * 5) is still just O(n)