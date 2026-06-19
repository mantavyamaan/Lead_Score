A Lead Score is a numerical value assigned to a potential customer (lead) that indicates how likely they are to become a paying customer.

It helps sales and marketing teams prioritize their efforts by focusing on the most promising leads first.

This model extends and refines the reference framework into a production-grade system suitable for multinational corporations with complex sales cycles, multiple product lines, and global operations

Every raw variable is normalised to x ̃∈[0,1]. Hard gates g_k∈{0,1}are binary kill switches. Soft modifiers m_j∈[0.5,1.0] apply penalties but never zero out a lead entirely. The final score lives on a 0–100 scale.



The Core Mathematical Concepts used are:

## Beta-Adjusted Confidence Interval (BCI)

The problem: An employee who completed 1 out of 1 tasks has a 100% completion rate. An employee who completed 98 out of 100 has a 98% rate. Raw percentages say the first employee is better — but we have almost no evidence for that claim.

Instead of using the raw fraction (successes ÷ total), model the rate as a Beta distribution and extract the 5th percentile. This is the value you can be 95% confident the true rate is at least this high.

$$BCI(s,n)= B^{-1}(0.05, s+1, n-s+1)$$

Where:

- s= no of successes

- n= number of total attempts

- $B^{-1}$= inverse regularised incomplete Beta function.

|Scenario |Raw Rate|BCI Score|
| --- | --- | --- |
1 out of 1 |100%|~33%
10 out of 10|100%|~78%
50 out of 55|91%|~83%
98 out of 100|98%|~94%
980 out of 1000|98%|~97%

More data → the BCI converges toward the raw rate. 

Less data → heavy automatic penalty. 

This rewards employees with a solid, well-evidenced track record over those with a thin but technically perfect one.

<br>

## KDE Percentile Normalisation (KPN)

The problem: Manager A rates everyone 4-5 out of 5. Manager B rates everyone 2-3 out of 5. A rating of "3" from Manager A actually indicates poor performance; a "3" from Manager B is above average. Raw scores are not comparable across evaluators.

$$KPN_j(x)=\frac{1}{n} \sum_{i=1}^{n} \Phi\!\left( \frac{x - x_i}{h_j}\right)$$

Where:

- $x$ = the raw rating given to the employee
- $x_i$ = the $i$-th historical rating by evaluator $j$
- $\Phi$ = standard normal cumulative distribution function
- $h_j$ = bandwidth, calculated using Silverman's rule:

$$h_j=0.9\times\min\left(\hat{\sigma}_j,\frac{IQR_j}{1.34}\right)\times n_j^{-1/5}$$

Intuitive effect:

A "3/5" from a strict manager (whose average rating is 2.5) might become the 85th percentile → score ≈ 0.85

A "3/5" from a lenient manager (whose average rating is 4.0) might be the 20th percentile → score ≈ 0.20

Everyone is now on the same scale regardless of who evaluated them.

<br>

## Exponentially Weighted Moving Average (EWMA)

The problem An employee performed terribly two years ago but has been excellent for the last six months. Should old performance drag them down equally?


The decay parameter:

$$\lambda = 0.85$$

controls how quickly old data fades.


$$
\bar v=\frac{\sum_{t=1}^{T}\lambda^{T-t}v_t}{\sum_{t=1}^{T}\lambda^{T-t}}$$

$$\sigma_{EWMA}^{2}=(1-\lambda)\sum_{t=1}^{T}\lambda^{T-t}
\left(v_t-\bar v\right)^2$$

Where:

- $v_t$ = the measurement at time period $t$
- $T$ = the most recent period
- $\lambda = 0.85$

Intuitive effect with $\lambda = 0.85$:

| Period (most recent first) | Weight |
|------------|------------|
| Current month | 100% (reference) |
| 1 month ago | 85% |
| 2 months ago | 72% |
| 3 months ago | 61% |
| 6 months ago | 38% |
| 12 months ago | 14% |


An improving employee is rewarded; a declining one is penalised — even if their lifetime average is identical.

<br>

## Copula Multi-Rater Aggregation (CMA)

The problem: When multiple raters (manager, peer, tech lead) each assess the same trait, simply averaging their KPN-normalised scores ignores the fact that some raters tend to agree with each other.

Correlated raters should collectively carry less weight than independent ones.

Normalise each rater's score:

$$u_i = KPN_i(r_i)$$

Transform to standard normal:

$$z_i = \Phi^{-1}(u_i)$$

Estimate the correlation matrix $R$ from historical multi-rater data.

Compute the precision-weighted aggregate:

$$\bar z=\frac{\mathbf{1}^{T}R^{-1}}
{\mathbf{1}^{T}R^{-1}\mathbf{1}}$$

Back-transform:

$$CMA=\Phi(\bar z)$$

Intuitive effect:

If two raters always agree (correlation ≈ 1), they collectively count almost as one rater.

If three raters are completely independent (correlation ≈ 0), each carries full weight, and the aggregate is much more precise.


## Huber-Robust Sub-Composite (HRC)

The problem: When combining multiple variables into a sub-composite with a weighted average, a single extreme outlier variable can distort the entire composite.

For example, if one variable has a data entry error giving it a value of 0.01 while everything else is around 0.80, a simple weighted average drops significantly.



$$HRC=\arg\min_{\theta}\sum_i w_i\cdot\rho_\delta(x_i-\theta)$$

Where the Huber loss function is:

$$\rho_\delta(a)=\begin{cases}\frac{1}{2}a^2&|a|\le\delta\\[8pt]\delta\left(|a|-\frac{1}{2}\delta\right)&|a|>\delta\end{cases}$$

With

$$\delta = 0.15$$

### How to solve it (IRLS — Iteratively Reweighted Least Squares)

Start with the ordinary weighted average:

$$\theta^{(0)}=\sum w_i x_i$$

Update weights to down-weight outliers:

$$w_i^{(t)}=w_i\times\min\left(1,\frac{\delta}{|x_i-\theta^{(t)}|}\right)$$

Recompute the weighted average with updated weights:

$$\theta^{(t+1)}=\frac{\sum w_i^{(t)}x_i}{\sum w_i^{(t)}}$$

Repeat until:

$$\left|\theta^{(t+1)}-\theta^{(t)}\right|<
10^{-6}$$

Intuitive effect: 

Variables within 0.15 of the composite centre contribute at full weight.

Variables farther away have their influence reduced proportionally.

This makes the composite resistant to single-variable data errors.


## Logarithmic Normalisation (LN)

The problem: Count-based variables (number of initiatives, certifications, client mentions) have diminishing returns.

Going from 0 to 3 initiatives is a meaningful signal of proactivity.

Going from 30 to 33 is noise.

Apply a natural logarithm transformation with a saturation cap:

$$LN(x,c)=\min\left(\frac{\ln(1+x)}{\ln(1+c)},1\right)$$

Where $c$ is the saturation point (the count at which the score maxes out).

Intuitive effect (with $c=10$):

| Raw Count | LN Score |
|------------|------------|
| 0 | 0.00 |
| 1 | 0.29 |
| 3 | 0.58 |
| 5 | 0.75 |
| 10 | 1.00 |
| 20 | 1.00 (capped) |

The first few count a lot.

Additional ones matter less and less.


## Exponential Decay Penalty (EDP)

The problem: For variables where higher values indicate worse outcomes (days late, defects per deliverable, response time), the damage from the first unit is much greater than from additional units.


Being 1 day late on a deliverable is a serious issue.

Being 10 days late vs. 11 days late — the marginal damage is much smaller because the harm is already done.


$$EDP(x,k)=e^{-k x}$$

Where $k$ controls the decay rate.

Intuitive effect (with $k=0.15$ for delays):

| Days Late | EDP Score |
|------------|------------|
| 0 | 1.00 |
| 2 | 0.74 |
| 5 | 0.47 |
| 10 | 0.22 |
| 20 | 0.05 |

