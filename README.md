# con3916-lab07-bootstrapping
# Estimation & Bootstrapping — Quantifying Uncertainty Without a Formula

## Objective

I wanted to measure how uncertain an estimate is, both with textbook formulas and with the bootstrap, and to find out when each approach can be trusted.

## Methodology

- Loaded the UCI Adult Census Income data (32,561 respondents).
- Separated the standard deviation of the data from the standard error of an estimate, and checked the s/sqrt(n) relationship across four sample sizes.
- Built parametric confidence intervals from the Central Limit Theorem, and plotted sampling distributions of the mean at increasing sample sizes for age and for capital gains.
- Ran the bootstrap by hand with a resampling loop, then repeated it with `scipy.stats.bootstrap`.
- Bootstrapped the median age to see how the bootstrap behaves on heavily tied, whole-number data.
- Compared percentile and BCa intervals on a heavily skewed variable (positive capital gains).
- Bootstrapped the difference in mean weekly hours between two groups.
- Asked an AI to write a reusable bootstrap-interval function, revised my prompt once, and checked the function against my own intervals.
- Ran a coverage simulation on lognormal data at n = 50 to compare the parametric interval and the percentile bootstrap, for both the mean and the median.

## Key Findings

- **SD and SE answer different questions.** The standard deviation describes the spread of the data and stays the same as the sample grows. The standard error describes the precision of the estimate, and it shrinks in line with s/sqrt(n).
- **The CLT doesn't arrive at the same speed for every variable.** It converged quickly for age, but slowly for capital gains, which are heavily skewed.
- **A bootstrap SE of zero can be correct.** The bootstrap SE of the median of 32,561 whole-number ages came out to exactly 0.0000. Every resample returned the same median, because with that many ties the median is pinned in place. The bootstrap was reporting a real property of the data, not failing.
- **Percentile and BCa intervals differ more as skew increases.** On the skewed capital gains variable, BCa shifted the interval to correct for skew in the bootstrap distribution.
- **The AI-written function was only trustworthy after I checked it.** My first prompt left out details the AI had to guess. I revised the prompt once, then confirmed the function against intervals I had already computed myself.
- **Coverage depends on the statistic.** For the mean, the nominal 95% parametric interval covered [YOUR VALUE]% and the percentile bootstrap covered [YOUR VALUE]%. That gap is within the Monte Carlo standard error of about 1.3 points, so the two methods can't be told apart here, and both under-cover slightly. For the median, the mean-centred parametric interval covered only about [YOUR VALUE]%, while the bootstrap stayed near its nominal level.
