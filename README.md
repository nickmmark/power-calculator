# power-calculator
simple html/js sample size calculator for clinical trials

## Statistical methods
All methods test H₀: p₁ = p₂. The treatment arm rate is p₂ = baseline − Δ. Output N is total patients; equal allocation is assumed throughout, so N = 2 × n per arm, rounded up to the nearest even integer.

Shared notation:

$$\bar{p} = \frac{p_1 + p_2}{2}, \quad z_\alpha = \Phi^{-1}\!\left(1 - \frac{\alpha}{2}\right), \quad z_\beta = \Phi^{-1}(\text{power})$$

---

### Uncorrected z

$$n = \frac{\Bigl(z_\alpha\sqrt{2\bar{p}(1-\bar{p})} + z_\beta\sqrt{p_1(1-p_1)+p_2(1-p_2)}\Bigr)^2}{(p_1-p_2)^2}$$

Standard two-proportion z-test. Slightly liberal at small N.

---

### Yates continuity-corrected z

$$n = \frac{n_0}{4}\left(1 + \sqrt{1 + \frac{2}{n_0\,|p_1-p_2|}}\right)^2$$

where $n_0$ is the uncorrected per-arm estimate. More conservative than uncorrected; converges toward Fisher at small N.

---

### Fleiss correction

$$n = \frac{n_0}{4}\left(1 + \sqrt{1 + \frac{4}{n_0\,|p_1-p_2|}}\right)^2$$

Same structure as Yates with a different inner constant. Produces slightly different estimates when $p_1$ and $p_2$ are asymmetric around 0.5.

---

### Kelsey (odds ratio–based)

$$\text{OR} = \frac{p_2(1-p_1)}{p_1(1-p_2)}, \qquad n = \frac{(z_\alpha + z_\beta)^2}{(\ln\text{OR})^2\,\bar{p}(1-\bar{p})}$$

Appropriate when the planned analysis is logistic regression. Converges with the uncorrected z-test at low event rates; diverges where OR and absolute risk difference decouple.

---

### Fisher's exact (iterated enumeration)

$$\text{Power}(n) = \sum_{a=0}^{n}\sum_{c=0}^{n} \binom{n}{a}p_1^a(1-p_1)^{n-a} \cdot \binom{n}{c}p_2^c(1-p_2)^{n-c} \cdot \mathbf{1}\bigl[p_\text{Fisher}(a,c) < \alpha\bigr]$$

Returns the smallest even N such that Power(N/2) ≥ target. The only fully exact method. Computationally expensive; caps at N = 4,000.

## Choosing a method
SituationRecommended methodGeneral use, large N expectedUncorrected zSmall expected N (< ~100/arm)Yates or FleissAnalysis will use logistic regressionKelseyRegulatory or exact test requirementFisher's exactComparing methods / sensitivity checkRun all four
