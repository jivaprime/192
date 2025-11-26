# 196
196, SDI(Symmetry Defect Index)

## Probing 196 with an “Asymmetry Density” Metric (SDI)

### 1) A quick introduction to 196

The number **196** is one of the most famous candidates for a *Lychrel number*.
The experiment is simple:

1. Take a natural number (n) in base 10.
2. Reverse its decimal digits.
3. Add the reversed number to the original.
4. Repeat.

Many numbers eventually land on a **palindrome** (a number that reads the same forwards and backwards).

For example, 89 behaves like this:

* 89 + 98 = 187
* 187 + 781 = 968
* 968 + 869 = 1837
* … (after more iterations) …
* At some point, a palindromic number appears.

196 is different. So far, no one has ever found a palindrome in the reverse-and-add chain of 196, despite pushing computations extremely far. It is therefore treated as a **Lychrel candidate**:

> Does 196 *never* reach a palindrome under reverse-and-add in base 10?

Mathematically, we still have no proof either way.

---

### 2) The SDI metric (Symmetry Defect Index)

Instead of only asking *“Does 196 ever become a palindrome?”*, I wanted to look at something more dynamic:

> As we iterate reverse-and-add on 196,
> **how does the symmetry or randomness of its digits evolve over time?**

To do this, I used a simple ad-hoc metric called **SDI – Symmetry Defect Index**.
It’s not meant to be a deep theoretical object, just a crude “asymmetry sensor.”

#### 2.1 Intuitive definition

Take an integer (n) and write it in base 10 as a string.

1. Split the digits into two halves:

   * the **left half**,
   * and the **right half**, but *reversed*, so that each pair of digits faces its “mirror”:

   Example: ( n = 1234567 )

   * digits: `"1234567"`, length = 7 → pairs = 3
   * left half: `"123"`
   * right half (reversed): last 3 digits `"567"` (which corresponds to 7,6,5 mirrored against 1,2,3)

   So the digit pairs are: (1,7), (2,6), (3,5).

2. For each pair ((d_L, d_R)), compare them in two very simple ways:

   * (d_L \bmod 2) vs (d_R \bmod 2) → are they both even/odd?
   * (d_L \bmod 5) vs (d_R \bmod 5) → which “bucket” 0–4 do they fall into?

3. Define the contribution of one pair as:
   [
   \text{pair_score} =
   \big|,(d_L \bmod 2) - (d_R \bmod 2),\big|
   +
   \big|,(d_L \bmod 5) - (d_R \bmod 5),\big|.
   ]

   * If the two digits behave similarly under mod 2 / mod 5, this is small (close to 0).
   * If they behave very differently, it can go up to 5.

4. Sum this value over all pairs to get a raw SDI.
   Finally, divide by the number of pairs to get an *average per pair*:

   [
   \text{Normalized SDI} = \frac{\text{SDI}}{\text{#pairs}}.
   ]

In the plots, I call this **“Asymmetry Density”**.

#### 2.2 Interpretation

This is a very rough heuristic, but the intuition is:

* **Lower normalized SDI**
  → the left and right halves have similar parity and mod-5 patterns
  → the number is **more symmetric / more structured**.

* **Higher normalized SDI**
  → the two halves often disagree in mod-2 / mod-5 behaviour
  → the number looks **more asymmetric / closer to random**.

If you simulate purely random decimal digits, the average normalized SDI tends to cluster around **≈ 2.1**. In the plots, this value is shown as a gray dotted line and used as a **“theoretical randomness” reference level**.

In addition, I introduced an informal threshold around **1.6**, marked as the **“Zombie Line.”**
Empirically, if a trajectory sits *well below* this line and stays there, it looks like a *frozen* or *dead* state; above it, the number still looks more “alive” and fluctuating.

---

### 3) Experimental setup and overview of results

#### 3.1 Extreme test for 196 (50,000 steps)

* Starting number: **196**
* Operation: base-10 reverse-and-add
* Maximum iterations: **50,000 steps**
* SDI sampling: computed every 100 steps to save time
* Environment: Python big integers + string operations

Python 3.11 introduced a safety limit on converting very large integers to strings (about 4300 digits). Since the reverse-and-add chain for 196 quickly exceeds this, I explicitly disabled the limit with:

```python
sys.set_int_max_str_digits(0)
```

By the time we reach 50,000 steps, the number of digits in the 196 chain is about **20,000 digits**. In magnitude, that’s roughly on the order of

[
10^{20{,}000},
]

which is absurdly larger than anything we normally encounter (for comparison, the estimated number of atoms in the observable universe is ~(10^{80})).

So, in SDI terms, we are tracking:

> How the asymmetry density of 196 behaves
> **all the way up to about (10^{20{,}000})** in the reverse-and-add chain.

#### 3.2 Comparison with 89

To check whether SDI actually captures “symmetry” in a reasonable way, I used **89** as a control.

* 196: normalized SDI for steps 0–200 (and separately up to 50k).
* 89: normalized SDI up to the step where it finally reaches a palindrome.
* Same SDI definition and almost identical code.

Since 89 is known to eventually hit a palindrome, we expect:

> Its SDI should start somewhat noisy and then **collapse to 0**
> when a perfectly symmetric number appears.

The comparison between 196 and 89 makes the behaviour very clear.

---

### 4) The two figures

In the Reddit post I plan to include two plots:

1. **Figure 1 – Extreme Lychrel Test: 196 up to 50,000 steps**

   * x-axis: step (0–50,000)
   * y-axis: normalized SDI (Asymmetry Density)
   * Orange line: 196’s SDI trajectory
   * Red dashed line: linear trend line (slope ≈ 0.000007)
   * Gray dotted line: theoretical randomness (~2.1)
   * Blue dashed line: “Zombie Line” (~1.6)

2. **Figure 2 – Normalized SDI: 196 vs 89 (early steps)**

   * x-axis: step (roughly 0–200)
   * y-axis: normalized SDI
   * Orange line: 196
   * Blue line: 89
   * Red dashed line: trend line for 196 (slope ≈ 0.00006, basically flat)
   * Gray dotted line: theoretical randomness (~2.1)

---

### 5) Analysis of the plots and conclusions

#### 5.1 Long-term behaviour of 196 (Figure 1)

A few things stand out in the 50k-step plot for 196:

1. **Range of values**

   * The normalized SDI mostly lives between **≈ 1.1 and 2.2**.
   * There is **no sign** of it collapsing towards 0 (which would indicate a perfectly symmetric state).

2. **Relation to the randomness line (2.1)**

   * Some spikes go up to around 2.1 or slightly above,
     but the bulk of the distribution sits **somewhat below** this line, roughly in the 1.3–1.9 range.
   * So the digits are *not* behaving like fully random noise; there is still residual structure.

3. **Zombie Line (~1.6)**

   * A large portion of the trajectory hovers around **1.6**,
     and the process does *not* drop far below this threshold and stay there.
   * In other words, 196 does **not** relax into a “cold”, highly symmetric, low-SDI state.
     It remains in a mid-level disorder band.

4. **Trend line**

   * The global linear fit over 50,000 steps has a tiny positive slope (~(7 \times 10^{-6})).
   * That corresponds to only about 0.3–0.4 increase over the full 0–50k range.
   * Visually, the trend is almost flat: if anything, 196 drifts *very slightly* toward higher disorder over time, but the effect is weak.

Overall, the 196 trajectory looks like this:

> It does **not** heal into symmetry (SDI → 0),
> it does **not** explode into full randomness (SDI ≈ 2.1),
> instead it spends a very long time in a **“zombie band”** above the 1.6 line,
> keeping a moderate level of asymmetry that refuses to die out.

#### 5.2 196 vs 89: healing vs zombie (Figure 2)

The second figure (196 vs 89) is a nice sanity check for SDI.

* **89 (blue)**

  * Starts with SDI values around 2–3, clearly noisy and disordered.
  * As the reverse-and-add iterations continue, the trajectory *visibly drifts downward*.
  * Finally, SDI drops sharply to **0**, and the curve ends there.
    That drop corresponds exactly to the step where a **palindrome** appears.
  * From the SDI point of view, 89 is:

    > a “healing” sequence: disordered at first, then converging to perfect symmetry.

* **196 (orange)**

  * Has some large early spikes (up to ~3.5),
    but quickly settles into the 1.2–2.2 band.
  * From there on, it just **jiggles** inside that band and refuses to move decisively up or down.
  * The trend line is basically horizontal; there is no clear tendency toward SDI = 0.
  * From the SDI perspective, 196 shows:

    > no sign of healing, and no sign of total meltdown either.

So SDI successfully distinguishes:

* “normal” reverse-and-add numbers that **eventually become palindromes**
  (like 89 → SDI collapses to 0), and
* the 196 chain, which is **stuck in a mid-level asymmetry state** with no obvious route to symmetry.

#### 5.3 Experimental conclusions (not a proof!)

None of this is a mathematical proof of anything about 196.
But from an *experimental* / numerical perspective, we can say:

1. Pushing the reverse-and-add chain of 196 to **50,000 steps** (about **20,000 digits**) and measuring SDI along the way, we see:

   * no approach toward SDI = 0,
   * no drift toward fully random behaviour either,
   * instead, a persistent band of mid-level asymmetry around the Zombie Line.

2. Compared with a “normal” case like 89:

   * 89’s SDI trajectory behaves exactly as expected for something that *does* reach a palindrome:
     disordered at first, then eventually collapsing to 0.
   * 196 shows fundamentally different long-term behaviour: it stays in a **chronic, noisy, mid-disorder state**.

From the SDI viewpoint, 196 looks less like a number that is “on its way” to a palindrome, and more like:

> a **Lychrel-style zombie sequence**
> that keeps wandering forever in a stable band of moderate asymmetry.

Of course, this is all under very specific assumptions:

* base 10,
* standard reverse-and-add,
* SDI defined via mod-2 and mod-5 comparisons,
* and a finite horizon of 50k steps / ~20k digits.

A natural next step would be to test:

* other starting values (more Lychrel candidates and non-candidates),
* other bases,
* and other symmetry/randomness indicators (variants of SDI, entropy measures, autocorrelation, etc.).

If similar “zombie-band” behaviour shows up repeatedly across those variations,
we might be looking at an interesting **empirical rule of thumb** for Lychrel-like dynamics, not just a one-off curiosity of 196.

