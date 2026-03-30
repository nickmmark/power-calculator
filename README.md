# power-calculator
simple html/js sample size calculator for clinical trials

## Statistical methods
### Uncorrected Z-test
```
p_bar = (p1 + p2) / 2

n_per_arm = ( z_alpha * sqrt(2 * p_bar * (1 - p_bar))
            + z_beta  * sqrt(p1*(1-p1) + p2*(1-p2)) )^2
            / (p1 - p2)^2
```

### Yates
Start from uncorrected n_per_arm = n0, then:
```math
n_per_arm = (n0 / 4) * (1 + sqrt(1 + 2 / (n0 * |p1 - p2|)))^2
```

### Fleiss
Same starting point as Yates, different inner constant:
```math
n_per_arm = (n0 / 4) * (1 + sqrt(1 + 4 / (n0 * |p1 - p2|)))^2
```

### Kelsey (OR based)
```math
OR    = (p2 * (1 - p1)) / (p1 * (1 - p2))
p_bar = (p1 + p2) / 2

n_per_arm = (z_alpha + z_beta)^2
            / (ln(OR)^2 * p_bar * (1 - p_bar))
```

### Fisher's Exact (enumerated)
For each candidate N, compute power by full enumeration:
```math
power = sum over a in 0..n:

          sum over c in 0..n:
            P(X1 = a | n, p1) * P(X2 = c | n, p2)
            * I[ fisher_pval(a, n-a, c, n-c) < alpha ]

# Return smallest even N where power >= target
```

## Choosing a method
SituationRecommended methodGeneral use, large N expectedUncorrected zSmall expected N (< ~100/arm)Yates or FleissAnalysis will use logistic regressionKelseyRegulatory or exact test requirementFisher's exactComparing methods / sensitivity checkRun all four
