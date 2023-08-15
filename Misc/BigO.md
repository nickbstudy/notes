Big O notation can objectively describe the efficiency of code without the use of concrete units.  It focuses on how the time and space requirements scale, and generally calculates for the worst case scenario.

Lines that only run once (constants) can be ignored as they don't affect the value.

Perfect display here:  [Part 1](https://www.youtube.com/watch?v=HfIH3czXc-8) [Part 2](https://www.youtube.com/watch?v=zo7YFqw5hNw)

Basically ignore any simple aritmetic involving numbers, not arguments. `O(10n * 5)` simplifies to `O(n)`

---

### Order of complexity

1. **Constant** - O(1)
2. **Logarithmic** - O(log(n))
3. **Linear** - O(n)
4. **Loglinear/Linearithmic** - O(n*log(n))
5. **Polynomial** - O(n^c) - n^2 known as quadratic, n^3 is cubed
6. **Exponential** - O(c^n)
7. **Factorial** - O(n!)

###[Big O Cheat Sheet](https://www.bigocheatsheet.com/)

#### Big O = worst case calculation
#### Big Theta Θ = average
#### Big Omega Ω = best case

One way to remember - omega starts and ends at the bottom (least time taken), theta has a dash in the middle (average), and writing an O you start and finish at the top (most time).
