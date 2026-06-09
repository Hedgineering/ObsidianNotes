If $f(x)$ is continuous, then it has a *derivative*
$$
\frac{d}{dx}f(x)  
\equiv  
f'(x)  
\equiv  
\lim_{h \to 0}  
\frac{f(x+h)-f(x)}{h}
$$
Then for small $h$,  
  
$$
f'(x) \approx \frac{f(x+h)-f(x)}{h}  
$$  
  and  
$$
f(x+h) \approx f(x) + h f'(x).  
$$

**Example:** Suppose you have a differential equation of a population growth model,

$$
f'(x) = 2f(x)
$$

with

$$
f(0) = 10.
$$

Let's "solve" this using a fixed-increment time approach with

$$
h = 0.01.
$$

(This is known as *Euler's method*.)

By (1), we have

$$
f(x+h)
\approx
f(x) + h f'(x)
$$
If we plug in the definition of $f'(x)$ and factor out $f(x)$:
$$
f(x) + 2h f(x)
=
(1+2h)f(x).
$$

Similarly, we can write $2h$ as $h + h$, and substitute $x = x +h$ as seen below:

We've established from above that $f(x+h) \approx (1+2h)f(x)$, 
so if we substitute $x = x +h$  we can see that  $f((x+h)+h) \approx (1+2h)f(x+h)$
then substitute $f(x+h) \approx (1+2h)f(x)$ to see $f((x+h)+h) \approx (1+2h)f(x+h) = (1+2h)(1+2h)f(x) = (1+2h)^2f(x)$.

In general,

$$
f(x+ih)
\approx
(1+2h)^i f(x),
\qquad
i = 0,1,2,\ldots
$$

though the approximation may deteriorate as $i$ gets large.

---

Plugging in $f(0)=10$ and $h=0.01$, we have

$$
f(0.01i)
\approx
10(1.02)^i,
\qquad
i = 0,1,2,\ldots
$$

Now, I happen to know that the true solution to the differential equation is

$$
f(x) = 10e^{2x}.
$$
You can see [[Analytical Population Diff Eq Solution]] for the full analytical solution.

So the approximation makes sense since for small $y$ (as per Taylor approximations),

$$
e^y
=
\sum_{\ell=0}^{\infty}
\frac{y^\ell}{\ell!}
\approx
1+y
\approx
(1+y)^i
\quad \text{for small } i.
$$

$$
\begin{array}{c|ccccccc}
x = ih = 0.01i
& 0
& 0.01
& 0.02
& 0.03
& 0.04
& \cdots
& 0.10
\\
\hline
\text{approx } f(x)\approx 10(1.02)^i
& 10.00
& 10.20
& 10.40
& 10.61
& 10.82
& \cdots
& 12.19
\\
\text{true } f(x)=10e^{2x}
& 10.00
& 10.20
& 10.41
& 10.62
& 10.83
& \cdots
& 12.21
\end{array}
$$
Not bad for small $i$ values $\hspace{0.25cm} \square$   