# A Really Good Combined Generator (L'Ecuyer, 1999)

A combined multiple recursive generator (CMRG) that combines two recursive generators with different moduli to produce high-quality pseudorandom numbers.

## Initialization

Initialize the following six seed values:

$$
X_{1,0},\; X_{1,1},\; X_{1,2},\;
X_{2,0},\; X_{2,1},\; X_{2,2}
$$

For

$$
i \ge 3,
$$

compute the sequences as follows.

## Generator 1

$$
X_{1,i}
=
\left(
1,\!403,\!580\,X_{1,i-2}
-
810,\!728\,X_{1,i-3}
\right)
\bmod
\left(2^{32}-209\right)
$$

## Generator 2

$$
X_{2,i}
=
\left(
527,\!612\,X_{2,i-1}
-
1,\!370,\!589\,X_{2,i-3}
\right)
\bmod
\left(2^{32}-22,\!853\right)
$$

## Combine the Two Generators

$$
Y_i
=
\left(
X_{1,i}
-
X_{2,i}
\right)
\bmod
\left(2^{32}-209\right)
$$

## Normalize to $[0,1)$

$$
R_i
=
\frac{Y_i}{2^{32}-209}
$$

## Algorithm

1. Initialize the six seed values.
2. Generate the next value of Generator 1.
3. Generate the next value of Generator 2.
4. Compute

   $$
   Y_i=(X_{1,i}-X_{2,i}) \bmod (2^{32}-209)
   $$

5. Normalize by dividing by the modulus:

   $$
   R_i=\frac{Y_i}{2^{32}-209}
   $$

6. Repeat for the desired number of random values.

## Notes

- This is a **Combined Multiple Recursive Generator (CMRG)**.
- Combining two generators significantly improves the period and statistical quality over a single LCG or MRG.
- The recurrence uses previous states (lags of 2–3), making it a higher-order recurrence rather than a first-order linear congruential generator.
- The output $R_i$ is a floating-point pseudorandom number in the interval

$$
[0,1).
$$