# Probability — Interview

Answers cover the published question list.

[← Back to hub](./7_special_algorithms.md)

### Related docs

- [Probability — Interview](./7_algo_probability_interview.md)

### Sections

- [Interview Practice](#interview-practice)

---

## Interview Practice

Questions from [Devinterview.io — Probability](https://devinterview.io/questions/machine-learning-and-data-science/probability-interview-questions/).

### Probability Basics

<details>
<summary>What is probability, and how is it used in machine learning?</summary>

### What is probability, and how is it used in machine learning?

**Definition:**

Probability assigns a number from 0 to 1 to how likely an outcome is. Machine learning uses those numbers as predictions, as training targets, and as a way to state uncertainty.

**How it works:**

- A model such as logistic regression outputs `P(y = 1 | x)`. A language model outputs a distribution over the next token.
- Training often minimizes a loss that is the negative log of the probability of the observed label.
- Bayes' rule updates a belief about parameters after data. Many regularizers are priors written as penalties.
- Calibration matters: a predicted 0.8 should be right about 80% of the time, not merely rank above a 0.2.
</details>

<details>
<summary>Define the terms sample space and event in probability.</summary>

### Define the terms sample space and event in probability.

**Definition:**

The sample space is the set of every outcome the experiment can produce. An event is a subset of that space, the outcomes you chose to group together.

**How it works:**

- A probability measure assigns each event a number from 0 to 1. The whole sample space has probability 1. The empty event has probability 0.
- The complement of an event is the rest of the sample space. `P(not A) = 1 − P(A)` when the space is exhaustive.
- Events combine with union (A or B) and intersection (A and B).
- Outcomes are the atoms. Events are what statements such as 'the die is even' refer to.

**Examples:**

One fair die has sample space `{1, 2, 3, 4, 5, 6}`. The event 'even' is `{2, 4, 6}` and has probability 1/2.
</details>

<details>
<summary>What is the difference between discrete and continuous probability distributions?</summary>

### What is the difference between discrete and continuous probability distributions?

**Definition:**

A discrete distribution puts probability on a countable set of values. A continuous distribution spreads probability over an interval so that any single point has probability zero.

**How it works:**

- Discrete: a probability mass function `P(X = x)` sums to 1 over the values. Examples are Bernoulli, binomial, and Poisson.
- Continuous: a probability density function integrates to 1. Probability of an interval is the area under the curve, not the height at a point. Examples are Gaussian and exponential.
- A density can be larger than 1. It is not itself a probability.
- Mixed data shows up as counts (discrete) and measurements (continuous). The model family has to match that.

**Examples:**

A coin is discrete: `P(heads) = 0.5`. A completion time in seconds is continuous: `P(time = 1.5000…) = 0`, while `P(1 ≤ time ≤ 2)` can be 0.4.
</details>

<details>
<summary>Explain the differences between joint, marginal, and conditional probabilities.</summary>

### Explain the differences between joint, marginal, and conditional probabilities.

**Definition:**

Joint probability is the chance that two statements are true together. Marginal is the chance of one statement after the other has been summed away. Conditional is the chance of one given that the other is known.

**How it works:**

- `P(A, B)` is the joint. `P(A) = Σᵦ P(A, B)` is the marginal for a discrete partner, and the integral plays that role in the continuous case.
- `P(A | B) = P(A, B) / P(B)` when `P(B) > 0`. Conditioning rescales the slice where B happened.
- The chain rule writes the joint as `P(A, B) = P(A | B) P(B)`.
- A model of `P(label | features)` is conditional. The training data's frequency table is closer to a joint, from which both can be derived.

**Examples:**

Rain and traffic: `P(rain, traffic) = 0.2`, `P(rain) = 0.25`. Then `P(traffic | rain) = 0.2 / 0.25 = 0.8`, and the marginal of traffic might be 0.3 after adding the no-rain slice.
</details>

<details>
<summary>What does it mean for two events to be independent?</summary>

### What does it mean for two events to be independent?

**Definition:**

Two events are independent when knowing one does not change the probability of the other.

**How it works:**

- The test is `P(A, B) = P(A) P(B)`, or equivalently `P(A | B) = P(A)` when `P(B) > 0`.
- Mutually exclusive events with positive probability are dependent: if one happened, the other did not.
- Pairwise independence of three events does not imply that all three are mutually independent. The triple joint must still factor.
- Naive Bayes assumes features are independent given the class, which is often false and still a useful approximation.

**Examples:**

Two fair coins: `P(first heads) = 0.5`, `P(both heads) = 0.25 = 0.5 × 0.5`. They are independent. 'First coin heads' and 'the two coins match' are not.
</details>

<details>
<summary>Describe Bayes' Theorem and provide an example of how it's used.</summary>

### Describe Bayes' Theorem and provide an example of how it's used.

**Definition:**

Bayes' theorem turns a known likelihood and a prior into the probability of the hypothesis after the evidence.

**How it works:**

- `P(H | D) = P(D | H) P(H) / P(D)`. `P(H)` is the prior, `P(D | H)` the likelihood, `P(H | D)` the posterior.
- `P(D)` is the total probability of the evidence, summed over hypotheses: `Σ P(D | Hᵢ) P(Hᵢ)`.
- A rare hypothesis stays unlikely unless the likelihood ratio is large. Base rates matter.
- Spam filters, medical tests, and Bayesian updates of model weights are the same identity with different names for H and D.

**Examples:**

A disease has prior 0.01. The test is positive for 90% of sick people and 5% of healthy people. `P(sick | positive) = (0.9 × 0.01) / (0.9 × 0.01 + 0.05 × 0.99) ≈ 0.15`. Most positive tests are still false alarms.
</details>

<details>
<summary>What is a probability density function (PDF)?</summary>

### What is a probability density function (PDF)?

**Definition:**

A PDF is a non-negative function whose area over the whole line is 1. The probability of an interval is the integral of the PDF over that interval.

**How it works:**

- The value `f(x)` is a density, not `P(X = x)`. That point probability is zero for a continuous variable.
- A taller peak means the values concentrate there. The curve may exceed 1 if the peak is narrow.
- The likelihood of a continuous observation in maximum likelihood is the density at that point, used for comparison, not as a probability.
- Histograms estimate a PDF by counting mass in bins and dividing by bin width.

**Examples:**

A uniform density on `[0, 0.5]` equals 2 everywhere inside the interval. `P(0.1 ≤ X ≤ 0.2) = 2 × 0.1 = 0.2`.
</details>

<details>
<summary>What is the role of the cumulative distribution function (CDF)?</summary>

### What is the role of the cumulative distribution function (CDF)?

**Definition:**

The CDF at `x` is `P(X ≤ x)`. It turns a distribution into a single non-decreasing curve from 0 to 1.

**How it works:**

- For a discrete variable it is the sum of masses up to `x`. For a continuous variable it is the integral of the PDF from the left up to `x`.
- Where the PDF exists, it is the derivative of the CDF. Jumps in the CDF are point masses.
- `P(a < X ≤ b) = F(b) − F(a)`. Quantiles are inverse lookups on the CDF.
- Empirical CDFs compare a sample to a theoretical distribution without choosing a bin width.

**Examples:**

A fair die has `F(3) = P(X ≤ 3) = 1/2` and `F(3.9) = 1/2` still, because nothing sits between 3 and 4.
</details>

### Probabilistic Models and Theories

<details>
<summary>Explain the Central Limit Theorem and its significance in machine learning.</summary>

### Explain the Central Limit Theorem and its significance in machine learning.

**Definition:**

The central limit theorem says that the average of many independent draws, suitably centered and scaled, approaches a normal distribution even when each draw is not normal.

**How it works:**

- The mean of the sample average is the true mean. Its variance is the single-draw variance divided by the sample size.
- The approximation improves as the sample grows, and it is poor for tiny samples or very heavy tails.
- That is why confidence intervals and many test statistics use a normal or a t curve for averages.
- Mini-batch gradient noise is often treated as roughly Gaussian for the same reason: each batch mean stacks many small contributions.

**Examples:**

Die rolls are uniform on 1..6, not bell-shaped. The average of 50 rolls still sits near 3.5 with a roughly normal histogram.
</details>

<details>
<summary>What is the Law of Large Numbers?</summary>

### What is the Law of Large Numbers?

**Definition:**

The law of large numbers says that the average of more and more independent draws from the same distribution settles on the expected value.

**How it works:**

- The weak form says the probability of a large gap between the average and the mean goes to zero. The strong form says the average converges with probability 1.
- Finite variance is the usual textbook condition. The draws must be identically distributed and not too dependent.
- It justifies replacing a theoretical mean with a long-run sample mean, including an empirical risk in place of the expected loss.
- It does not say the average becomes normal. That shape statement is the central limit theorem. It also does not say a short sample is already close.

**Examples:**

A fair coin's proportion of heads after 20 flips can sit at 0.7. After 20,000 flips it sits close to 0.5.
</details>

<details>
<summary>Define expectation, variance, and covariance.</summary>

### Define expectation, variance, and covariance.

**Definition:**

Expectation is the probability-weighted average of a random variable. Variance is the expected squared distance from that average. Covariance is how two variables move together.

**How it works:**

- `E[X] = Σ x P(X = x)` or the integral `x f(x) dx`. It is linear: `E[aX + bY] = a E[X] + b E[Y]` even when X and Y are dependent.
- `Var(X) = E[(X − E[X])²] = E[X²] − (E[X])²`. `Var(aX) = a² Var(X)`.
- `Cov(X, Y) = E[(X − E[X])(Y − E[Y])]`. Zero covariance does not by itself prove independence, except in special families such as jointly normal variables.
- A covariance matrix of features is what PCA diagonalizes. High variance in one feature can dominate a distance unless you scale.

**Examples:**

A fair die has expectation 3.5. Variance is about 2.92. Two independent dice have covariance 0, so the variance of their sum is about 5.83.
</details>

<details>
<summary>What are the characteristics of a Gaussian (Normal) distribution?</summary>

### What are the characteristics of a Gaussian (Normal) distribution?

**Definition:**

A normal distribution is the bell curve fixed by a mean `μ` and a variance `σ²`. It is symmetric, and the mean, median, and mode coincide.

**How it works:**

- The density is highest at `μ` and falls off as `exp(−(x − μ)² / (2σ²))`. About 68% of the mass lies within one standard deviation, about 95% within two, about 99.7% within three.
- The sum of independent normals is normal. A scaled normal is normal. That closure is why noise models add up cleanly.
- Standardizing `(X − μ) / σ` produces the standard normal, which is what z-tables use.
- Real errors are often only roughly normal. Heavy tails and hard bounds break the 68-95-99.7 picture.
</details>

<details>
<summary>Explain the utility of the Binomial distribution in machine learning.</summary>

### Explain the utility of the Binomial distribution in machine learning.

**Definition:**

A binomial count is the number of successes in a fixed number of independent yes-or-no trials that share one success probability.

**How it works:**

- `P(K = k) = C(n, k) pᵏ (1 − p)ⁿ⁻ᵏ`. The mean is `np` and the variance is `np(1 − p)`.
- It models how many of n emails are spam, how many of n users click, or how many of n cross-validation folds pass, when each trial is a Bernoulli draw.
- Logistic regression models one trial. A binomial likelihood extends that to a count out of n at the same features.
- When n is large and p is not tiny, the count is approximately normal. When n is large and p is tiny, it is approximately Poisson.

**Examples:**

Ten independent users click with probability 0.2. The expected click count is 2, and the chance of zero clicks is `0.8¹⁰ ≈ 0.107`.
</details>

<details>
<summary>How does the Poisson distribution differ from the Binomial distribution?</summary>

### How does the Poisson distribution differ from the Binomial distribution?

**Definition:**

Poisson counts events in a fixed interval when they arrive independently at a constant average rate. Binomial counts successes in a fixed number of trials.

**How it works:**

- Poisson has one parameter `λ`, the mean count. Variance equals the mean. Probabilities are `e⁻λ λᵏ / k!`.
- There is no separate n. The interval can hold arbitrarily many events, though large k becomes unlikely.
- It is the limit of a binomial as n grows, p shrinks, and `np` stays at `λ`. That is why rare events in a large population look Poisson.
- Use Poisson for arrivals, faults per hour, or words in a window. Use binomial when the number of attempts is the thing you fixed.

**Examples:**

Calls average 4 per hour. `P(0 calls) = e⁻⁴ ≈ 0.018`. A binomial would need you to name the number of candidate callers first.
</details>

<details>
<summary>What is the relevance of the Bernoulli distribution in machine learning?</summary>

### What is the relevance of the Bernoulli distribution in machine learning?

**Definition:**

A Bernoulli distribution is one yes-or-no trial with success probability `p`. It is the likelihood underneath binary classification.

**How it works:**

- `P(X = 1) = p` and `P(X = 0) = 1 − p`. The mean is `p` and the variance is `p(1 − p)`.
- Logistic regression sets `p = σ(w · x + b)` and trains by the Bernoulli log-likelihood, which is binary cross-entropy.
- A sequence of independent Bernoulli trials with the same `p` is a binomial count. A different `p` per row is a set of separate Bernoulli draws.
- Accuracy throws away the probability. The Bernoulli likelihood uses the confidence, so 0.99 on a wrong label is punished harder than 0.51.

**Examples:**

A click model with `p = 0.2` has log-likelihood `log 0.2` on a click and `log 0.8` on a miss. Training nudges `w` so those logs grow on the observed labels.
</details>

### Applying Probability in Machine Learning

<details>
<summary>How do probabilistic models cope with uncertainty in predictions?</summary>

### How do probabilistic models cope with uncertainty in predictions?

**Definition:**

A probabilistic model returns a distribution over outcomes, not a single hard label. Uncertainty is the spread of that distribution.

**How it works:**

- Naive Bayes computes P(y | x) from a prior and per-feature likelihoods, then picks the class with the largest product. sklearn notes that predict_proba from naive Bayes is a weak probability estimate even when the class label is useful.
- A Gaussian mixture assigns each point a probability of coming from each component. GaussianMixture.predict returns the most probable component. The covariance is part of the uncertainty.
- LogisticRegression.predict_proba returns a probability per class. For three or more classes the documented rule is softmax.
- A point prediction hides the case where two classes are nearly tied. The distribution is what you threshold, rank, or refuse to act on.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
https://scikit-learn.org/stable/modules/mixture.html
</details>

<details>
<summary>In machine learning, what are Naive Bayes classifiers, and why are they naive?</summary>

### In machine learning, what are Naive Bayes classifiers, and why are they naive?

**Definition:**

Naive Bayes applies Bayes' theorem and then assumes that features are conditionally independent given the class. That independence assumption is the naive part.

**How it works:**

- sklearn writes P(y | x) proportional to P(y) times the product of P(x_i | y). The denominator is constant for a fixed input, so the predicted class is the argmax of that product.
- P(y) is the class frequency in the training set. Each P(x_i | y) is its own one-dimensional distribution, which is why the model needs relatively little data and avoids a joint table over all features.
- GaussianNB assumes a Gaussian likelihood and estimates mean and variance by maximum likelihood. MultinomialNB is the usual text model, with Lidstone or Laplace smoothing so a missing word is not a zero probability.
- The same page says the classifier often works well, including on documents and spam, and is fast, but it is a poor probability estimator.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
</details>

<details>
<summary>How does logistic regression utilize probability?</summary>

### How does logistic regression utilize probability?

**Definition:**

LogisticRegression is a probabilistic classifier. predict returns a class. predict_proba returns a probability for each class, and for a multiclass problem those probabilities come from softmax.

**How it works:**

- Regularization is on by default. C is the inverse strength: smaller C means stronger regularization, and C=inf is the unpenalized model. Since version 1.8 the penalty argument is deprecated; l1_ratio and C select L2, L1, elastic-net, or no penalty.
- The default solver is lbfgs, which the class docs limit to L2 or no penalty. saga is the solver that supports elastic-net. liblinear is binary unless wrapped in one-vs-rest.
- predict_log_proba returns the log of those class probabilities, ordered as classes_.
- The probabilities are what you threshold. The class label is the argmax of predict_proba, not a separate geometric rule.

**Sources:**

https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
</details>

<details>
<summary>What is the concept of entropy in information theory, and how does it relate to machine learning models?</summary>

### What is the concept of entropy in information theory, and how does it relate to machine learning models?

**Definition:**

Shannon entropy of a discrete distribution pk is H = -sum(pk * log(pk)). scipy.stats.entropy computes that, and with a second distribution it computes Kullback-Leibler divergence.

**How it works:**

- The docs describe H as the expected uncertainty of the outcomes. A fair coin has entropy 1 bit. A coin with probabilities 0.9 and 0.1 has entropy about 0.47 bits. The base selects the unit: 2 for bits, e for nats.
- Relative entropy D(pk || qk) is the extra surprise of believing qk when the true distribution is pk. Cross-entropy is H(pk) + D(pk || qk), equal to -sum(pk * log(qk)). entropy does not return cross-entropy in one call. Add the two calls.
- A classifier that emits class probabilities is scored by how much probability it put on the true class. That is cross-entropy against a one-hot pk. LogisticRegression.predict_log_proba is the log-probability side of that score.
- Decision splits and compression both prefer a distribution with less remaining uncertainty. scipy will normalize pk if it does not already sum to 1.

**Sources:**

https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.entropy.html
https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
</details>

<details>
<summary>Explain the relationship between Maximum Likelihood Estimation (MLE) and probability.</summary>

### Explain the relationship between Maximum Likelihood Estimation (MLE) and probability.

**Definition:**

Maximum likelihood picks the parameters that make the observed data most probable under the model. The likelihood is that probability, viewed as a function of the parameters.

**How it works:**

- GaussianNB estimates each class mean and variance by maximum likelihood. The Gaussian density on the naive Bayes page is the likelihood of one feature given the class.
- In a Gaussian mixture the labels of the components are hidden, so a single closed-form MLE is not available. EM iterates: assign probabilities of components, then refit the Gaussians to raise the likelihood. The mixture page says this converges to a local optimum.
- Ordinary least squares minimizes ||Xw - y||^2. That is a separate statement from calling it a Gaussian likelihood. The linear-model page states the residual sum of squares, not a probability.
- A maximum is not a posterior. A posterior multiplies the likelihood by a prior. Variational BayesianGaussian mixtures on the same page maximize a bound on evidence that includes priors, instead of the likelihood alone.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/mixture.html
</details>

<details>
<summary>How is probability used in Bayesian inference for machine learning?</summary>

### How is probability used in Bayesian inference for machine learning?

**Definition:**

Bayesian inference keeps a distribution over parameters or components. The posterior is proportional to the likelihood times the prior. A prediction averages over that posterior instead of plugging in one maximum-likelihood point.

**How it works:**

- Naive Bayes is the small version: P(y | x) proportional to P(y) P(x | y). P(y) is the prior class frequency. The page estimates it by maximum a posteriori counting.
- BayesianGaussianMixture replaces EM's pure likelihood with variational inference, which maximizes a lower bound on model evidence including priors. A small weight_concentration_prior pushes most weight onto a few components and drives the others toward zero.
- The same page says variational inference avoids some EM singularities by the prior, at the cost of bias, extra hyperparameters, and slower fitting. EM can diverge to infinite likelihood when a component has too few points.
- The prior is not the output. The output of predict is still a class or a component. The prior changes which parameters are plausible before the data are seen.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/mixture.html
</details>

### Practical Considerations of Probability in Machine Learning

<details>
<summary>How can assuming independence in probabilistic models lead to inaccuracies?</summary>

### How can assuming independence in probabilistic models lead to inaccuracies?

**Definition:**

Naive Bayes forces P(x_i | y, all other features) = P(x_i | y). If two features carry the same news, the product counts that news twice and the probability moves too far toward 0 or 1.

**How it works:**

- The page says this is why predict_proba should not be taken too seriously, even though the argmax class is often still right. Ranking classes can survive double-counting better than the numeric probability.
- The product form is also why the model is fast and needs less data: each feature is a separate one-dimensional estimate. The inaccuracy is the price of that split.
- A Gaussian mixture does not make that feature-independence claim. A full covariance models dependence inside a component. A diagonal covariance is the restricted case.
- Zero probabilities are a related failure. An unseen word makes a multinomial product zero unless alpha smoothing is on. That is a missing count, not dependence, and Laplace smoothing (alpha = 1) is the documented fix.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/mixture.html
</details>

<details>
<summary>Describe how to update probabilities using the concept of prior, likelihood, and posterior.</summary>

### Describe how to update probabilities using the concept of prior, likelihood, and posterior.

**Definition:**

Start with a prior P(y). Multiply by the likelihood of the evidence given y. Normalize. The result is the posterior P(y | evidence). The next observation uses that posterior as the new prior.

**How it works:**

- Naive Bayes writes this in one step: posterior proportional to prior times the product of per-feature likelihoods. The evidence P(x) is the normalizer and does not change the argmax.
- MultinomialNB's theta is a smoothed likelihood: (count + alpha) / (class total + alpha * n_features). Alpha is the prior count that keeps a zero from surviving into the posterior.
- EM is the same move with hidden component labels. The E-step writes a posterior responsibility of each component for each point. The M-step refits means and covariances as if those responsibilities were the data.
- Variational Bayesian mixtures add an explicit prior on the weights. A low concentration prior updates the posterior toward fewer live components.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/mixture.html
</details>

<details>
<summary>What strategies would you use to handle missing data in probabilistic models?</summary>

### What strategies would you use to handle missing data in probabilistic models?

**Definition:**

A missing feature is not a zero. Treating it as zero invents an observation. Omit it from that row's likelihood, or impute it from the distribution the model already estimates.

**How it works:**

- scipy.stats.ttest_1samp documents the same choice for NaNs: propagate makes the result NaN, omit drops NaNs, raise errors. Pick omit only when the remaining values are still a fair sample.
- MultinomialNB's alpha smoothing covers a word that never appeared in training. It does not cover a feature that failed to be recorded. Those are different holes.
- GaussianMixture can diverge when a component has too few points to estimate a covariance. The page says to regularize covariances. Dropping incomplete rows can create that shortage.
- A model that factors features, such as naive Bayes, can score a row from the features that are present, because the likelihood is a product. A full-covariance mixture cannot drop one coordinate without changing the density.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/mixture.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_1samp.html
</details>

<details>
<summary>How do you determine the significance of an observed effect using probability?</summary>

### How do you determine the significance of an observed effect using probability?

**Definition:**

Fix a null hypothesis and a threshold before looking at the p-value. scipy.stats.ttest_1samp tests that the mean of independent observations equals popmean. The p-value is the probability, under that null, of a statistic at least as extreme as the one observed.

**How it works:**

- The statistic is (mean(a) - popmean) / standard error. alternative is two-sided by default, or less, or greater.
- The scipy example tests a sample against popmean 0.5 at a 0.01 threshold. A p-value of about 0.018 does not reject. A normal sample whose mean is not 0.5 produces a tiny p-value and does reject. A true null is still rejected about 1% of the time at that threshold. The example shows 1 rejection in 100 uniform samples.
- Significance is not the size of the effect. A huge sample makes a tiny shift reject. Report the estimate and the interval with the decision.
- df is one less than the sample size along the tested axis. nan_policy controls missing values.

**Sources:**

https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_1samp.html
</details>

<details>
<summary>What are p-values and confidence intervals, and how are they interpreted?</summary>

### What are p-values and confidence intervals, and how are they interpreted?

**Definition:**

A p-value is the probability, under the null, of a result at least as extreme as the one observed. A confidence interval is a range of parameter values still compatible with the data at a chosen level. scipy.stats.ttest_1samp returns both: pvalue on the result, and confidence_interval(confidence_level).

**How it works:**

- The scipy notes say the bounds of a 95% interval are the popmean values at which the p-value would be 0.05. Under the test's assumptions, that interval is expected to contain the true mean in 95% of repeated samples. One example with a true mean of 1 contained it in 953 of 1000 replications.
- A p-value is not the probability that the null is true. The example rejects a true null about 1% of the time when the threshold is 0.01.
- alternative changes which tail counts. two-sided is the default. greater asks only whether the mean is larger than popmean.
- The interval is about the mean, not about the next observation. A wide interval means the sample did not pin the mean down.

**Sources:**

https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.ttest_1samp.html
</details>

### Specific Algorithms and Techniques

<details>
<summary>Describe how a probabilistic graphical model (PGM) works.</summary>

### Describe how a probabilistic graphical model (PGM) works.

**Definition:**

A probabilistic graphical model draws a random variable at each node and stores a conditional probability on each edge, so the joint distribution factors into small pieces instead of one giant table.

**How it works:**

- The Markov page's property is the usual reason an edge set is small: given the parents, a node does not depend on the rest of the graph. A chain is the graph with one parent each.
- Naive Bayes is a tiny graph: the class is the parent of every feature, and the features have no edges between them. That missing edge is the independence assumption, and the joint becomes the product on the naive Bayes page.
- A Gaussian mixture is a hidden-variable model. The component id is unobserved. EM fills in a probability for that hidden node and then updates the Gaussian parameters.
- Inference reads the conditionals in the direction the question asks. The graph does not by itself name an algorithm. Exact products work on a naive Bayes graph. A loopy graph needs an approximate pass.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/mixture.html
https://en.wikipedia.org/wiki/Markov_chain
</details>

<details>
<summary>Explain the concepts of Markov Chains and how they apply to machine learning.</summary>

### Explain the concepts of Markov Chains and how they apply to machine learning.

**Definition:**

A Markov chain is a sequence in which the probability of the next state depends only on the current state. The Markov page calls this memorylessness: given the present, the future and the past are independent.

**How it works:**

- A discrete-time chain jumps at discrete steps. A continuous-time chain does not. The state space can be countable. The page says the precise combination of discrete time and discrete state varies by author.
- The same page names Markov chain Monte Carlo as the simulation method that samples complicated distributions by running such a chain. Bayesian statistics, speech, and signal processing are listed applications.
- In a model this is a transition table: from state i, a row of probabilities sums to 1. Learning estimates those rows from observed transitions. Predicting one step reads the row of the current state.
- If the next click depends on the whole history, the chain is the wrong model until you fold that history into the state.

**Sources:**

https://en.wikipedia.org/wiki/Markov_chain
</details>

<details>
<summary>How do Hidden Markov Models (HMMs) use probability in sequential data modeling?</summary>

### How do Hidden Markov Models (HMMs) use probability in sequential data modeling?

**Definition:**

An HMM is a Markov chain on states you do not see, plus a probability of an observation given the hidden state. The Markov property still applies to the hidden state: the next state depends on the current hidden state, not on the earlier ones.

**How it works:**

- The visible sequence is speech, clicks, or letters. The hidden sequence is the label path. Two tables define the model: transitions between hidden states, and an emission distribution per state.
- The Markov page lists speech processing among Markov applications. The emission is the extra piece that makes the chain hidden: you score an observation with the emission of the state you hypothesize.
- Filtering asks for the probability of the current hidden state given observations so far. That is a posterior updated one step at a time, prior from the transition, likelihood from the emission.
- If the state is observed, you do not need an HMM. A plain transition table is enough.

**Sources:**

https://en.wikipedia.org/wiki/Markov_chain
https://scikit-learn.org/stable/modules/naive_bayes.html
</details>

<details>
<summary>What is Expectation-Maximization (EM) algorithm and how does probability play a role in it?</summary>

### What is Expectation-Maximization (EM) algorithm and how does probability play a role in it?

**Definition:**

EM fits a model with hidden labels by alternating two probability steps. sklearn's GaussianMixture implements it for a mixture of Gaussians.

**How it works:**

- The E-step computes, for each point, the probability it was generated by each component, given the current parameters.
- The M-step updates the means, covariances, and weights to raise the likelihood of the data given those probabilities. The page says repeating this converges to a local optimum.
- Initialization matters. The documented choices are k-means, k-means++, random data points, or a small perturbation of the global mean. A bad start can fail to converge.
- With too few points per component the covariance estimate can diverge to infinite likelihood. The page says to regularize covariances. BayesianGaussianMixture uses priors instead of pure likelihood and avoids some of those singularities.

**Sources:**

https://scikit-learn.org/stable/modules/mixture.html
</details>

### Coding Challenges

<details>
<summary>Write a Python function that calculates the probability of rolling a sum of S on two dice.</summary>

### Write a Python function that calculates the probability of rolling a sum of S on two dice.

**Definition:**

Two fair six-sided dice are independent, so each of the 36 outcomes has probability 1/36. Count the pairs (i, j) with i + j = S.

**How it works:**

- Sums below 2 or above 12 are impossible. The count rises from 1 at 2 to 6 at 7, then falls back to 1 at 12.
- This is an exact count, not a simulation. numpy.random.default_rng is for draws when you cannot enumerate. Here the space is small enough to enumerate.
- The function returns a fraction. Do not divide by 6. There are 36 ordered outcomes, not 6.

**Examples:**

```text
def two_dice(s):
    if not 2 <= s <= 12:
        return 0.0
    ways = sum(1 for i in range(1, 7) for j in range(1, 7) if i + j == s)
    return ways / 36
```

**Sources:**

https://numpy.org/doc/stable/reference/random/index.html
</details>

<details>
<summary>Implement a function that simulates a biased coin flip n times and estimates the probability of heads.</summary>

### Implement a function that simulates a biased coin flip n times and estimates the probability of heads.

**Definition:**

Draw n uniform values with a Generator and count how many fall below the heads probability p. The estimate is that count divided by n.

**How it works:**

- numpy.random.default_rng returns a Generator. rng.random() is uniform on [0, 1). An array of n draws is rng.random(n).
- The docs warn that these generators are for simulation, not for security. Do not use them as a secret coin.
- A seed makes the sequence reproducible. Without a seed, default_rng takes nondeterministic OS entropy and the next run differs.
- The estimate approaches p as n grows. For one run it is a random fraction. Report n with the estimate.

**Examples:**

```text
def estimate_heads(p, n, seed=None):
    rng = np.random.default_rng(seed)
    flips = rng.random(n)
    return float((flips < p).mean())
```

**Sources:**

https://numpy.org/doc/stable/reference/random/index.html
</details>

<details>
<summary>Code a Gaussian Naive Bayes classifier from scratch using Python.</summary>

### Code a Gaussian Naive Bayes classifier from scratch using Python.

**Definition:**

For each class, store the class frequency and the per-feature mean and variance. Predict by maximum likelihood under a Gaussian, times the class prior. sklearn's GaussianNB is this model and estimates those parameters by maximum likelihood.

**How it works:**

- The documented density is 1/sqrt(2 pi sigma^2) * exp(-(x - mu)^2 / (2 sigma^2)). In code, add log probabilities. A product of small densities underflows.
- The naive step is the sum of per-feature log-likelihoods. Features are not given a covariance.
- Variance must stay positive. A feature that is constant in a class needs a small floor, or the density divides by zero. The library's failure mode on the mixture page, infinite likelihood from a singular covariance, is the same hole.
- predict is the argmax over classes. The numeric posterior is that value normalized across classes. The naive Bayes page says not to trust the probability too much.

**Examples:**

```text
def log_gauss(x, mu, var):
    return -0.5 * (np.log(2 * np.pi * var) + (x - mu) ** 2 / var)
```

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
</details>

<details>
<summary>Simulate the Law of Large Numbers using Python: verify that as the number of coin tosses increases, the average of the results becomes closer to the expected value.</summary>

### Simulate the Law of Large Numbers using Python: verify that as the number of coin tosses increases, the average of the results becomes closer to the expected value.

**Definition:**

The average of independent fair coin tosses moves toward 1/2 as the number of tosses grows. Draw the tosses from a Generator and plot the running mean.

**How it works:**

- rng.integers(0, 2, size=n) yields 0 or 1. The expected value of one toss is 1/2. The running mean is the cumulative sum divided by the count.
- One path wiggles. A larger n pulls it closer. That is the claim to show, not a proof. Repeat with a fixed seed when you need the same path again.
- default_rng without a seed changes every run. Pass a seed to compare two sample sizes on the same stream family.
- These draws are for simulation. The numpy page says they are not a cryptographic coin.

**Examples:**

```text
rng = np.random.default_rng(0)
tosses = rng.integers(0, 2, size=10_000)
running = np.cumsum(tosses) / np.arange(1, 10_001)
```

**Sources:**

https://numpy.org/doc/stable/reference/random/index.html
</details>

<details>
<summary>Write a Python script that estimates the mean and variance of a dataset and plots the corresponding Gaussian distribution.</summary>

### Write a Python script that estimates the mean and variance of a dataset and plots the corresponding Gaussian distribution.

**Definition:**

The sample mean is the average. The sample variance is the average squared deviation. The Gaussian curve with that mean and variance is the density GaussianNB uses for one feature.

**How it works:**

- Use the population formula or the sample formula consistently and say which. A one-feature GaussianNB fit is maximum likelihood, which divides by n, not n - 1.
- Plot the density 1/sqrt(2 pi var) * exp(-(x - mean)^2 / (2 var)) on a grid. Do not plot the histogram and call it the density without scaling the counts.
- rng.standard_normal draws a standard Gaussian if you need synthetic data. Shift and scale it to the estimated mean and variance only when you are simulating, not when the dataset is already observed.
- A singular variance, all values equal, makes the density undefined. Floor it or refuse to plot, the same singularity the mixture page warns about.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://numpy.org/doc/stable/reference/random/index.html
</details>

### Scenario-Based Probability Questions

<details>
<summary>How would you use probability to design a recommendation system model?</summary>

### How would you use probability to design a recommendation system model?

**Definition:**

Score an item by the probability the user will engage, not by a raw count. A probabilistic classifier or a mixture turns history into P(engage | user, item).

**How it works:**

- MultinomialNB is the text version: a user profile is a bag of tokens, and the class is engage or not. Laplace smoothing covers an item the user has not seen.
- LogisticRegression.predict_proba returns a calibrated-looking score you can rank. For more than two outcomes, softmax is the documented map. Rank by the probability of the positive class, not by the hard label.
- A Gaussian mixture clusters users. Recommend from the component a user most probably belongs to, which GaussianMixture.predict returns.
- The naive Bayes page warns that the numeric probability can be over-confident when features repeat the same signal. Use it to rank, and check the ranking on held-out events.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
https://scikit-learn.org/stable/modules/mixture.html
</details>

<details>
<summary>Propose a method for predicting customer churn using probabilistic models.</summary>

### Propose a method for predicting customer churn using probabilistic models.

**Definition:**

Churn is a class. Fit LogisticRegression or a naive Bayes model on account features and read predict_proba for the churn class. Act on the probability, not only on the label.

**How it works:**

- The positive class probability is what a threshold policy uses. A high threshold spends retention budget on fewer customers. predict_proba is the documented method. For a binary problem it is the pair of class probabilities.
- GaussianNB fits a Gaussian per feature per class by maximum likelihood. Use it when features are numeric and roughly bell-shaped. MultinomialNB fits counts.
- Class imbalance changes P(y). The naive Bayes prior is the training frequency, so a rare churn class stays rare unless you reweight. LogisticRegression takes class_weight for that.
- Calibrate the decision on a held-out period. The naive Bayes page says the probability values themselves are not reliable even when the ranking is.

**Sources:**

https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
https://scikit-learn.org/stable/modules/naive_bayes.html
</details>

<details>
<summary>Discuss how you would use probabilities to detect anomalies in transaction data.</summary>

### Discuss how you would use probabilities to detect anomalies in transaction data.

**Definition:**

An anomaly is a point with low probability under a model of normal traffic. Fit that model on ordinary transactions and flag the tail.

**How it works:**

- GaussianMixture estimates a density. A point whose likelihood under every component is tiny is the candidate. The page warns that a component with too few points can make the likelihood infinite. Regularize the covariance before you trust a low score.
- EM assigns a responsibility, the probability of each component. A point that fits none of them has a low total density even if predict names a nearest component.
- A one-class rule on predict_proba is the wrong tool when every training row is labeled normal. There is no second class for logistic regression to contrast. The mixture does not need a fraud label.
- Threshold the score on a reviewed sample. A probability is not yet a fraud decision.

**Sources:**

https://scikit-learn.org/stable/modules/mixture.html
</details>

<details>
<summary>Describe how you might use Bayesian methods to improve the performance of a spam classifier.</summary>

### Describe how you might use Bayesian methods to improve the performance of a spam classifier.

**Definition:**

Spam filtering is the classic naive Bayes use on the sklearn page. Improve it by keeping the prior and the smoothing explicit, instead of a bare frequency count.

**How it works:**

- MultinomialNB estimates P(word | class) as (count + alpha) / (total + alpha * vocabulary). alpha = 1 is Laplace smoothing. It stops an unseen word from forcing the posterior to zero.
- The class prior P(spam) should match the mailbox, not a balanced toy set. The page estimates P(y) by class frequency, which is the MAP count.
- ComplementNB is the documented variant for imbalanced text. It uses statistics from the complement of each class and is aimed at text tasks where one class dominates.
- Do not treat predict_proba as a calibrated spam probability. The page says naive Bayes is a decent classifier and a bad probability estimator. Threshold on a labeled sample.

**Sources:**

https://scikit-learn.org/stable/modules/naive_bayes.html
</details>

<details>
<summary>Explain a situation where you would use Markov Chains for modeling customer behavior on a website.</summary>

### Explain a situation where you would use Markov Chains for modeling customer behavior on a website.

**Definition:**

Use a Markov chain when the next page depends on the current page and not on the path that got the user there. The state is the page. The transition is the probability of the next click.

**How it works:**

- The Markov page defines that memorylessness: given the present state, the future does not depend on the past. A session that really depends on the landing page violates it until that fact is folded into the state.
- Estimate each row by counts of observed transitions out of that page. A row must sum to 1. Predict the next page by reading the row. Predict a longer path by multiplying the matrix.
- Markov chain Monte Carlo, also on that page, is a different use: sampling a distribution by running a chain. A click model is the chain itself, not an MCMC sampler.
- If the next click depends on the last three pages, enlarge the state to that triple. The chain is then Markov on the enlarged state.

**Sources:**

https://en.wikipedia.org/wiki/Markov_chain
</details>

### Advanced Probability Concepts and Research

<details>
<summary>How has the advent of Quantum Computing influenced probabilistic algorithms?</summary>

### How has the advent of Quantum Computing influenced probabilistic algorithms?

**Definition:**

The random generators in the numerical stack you would call from these notes are still classical. numpy.random.default_rng uses PCG64, and the page says those generators are for statistical simulation, not for cryptography.

**How it works:**

- A Monte Carlo estimate draws independent classical samples and averages them. Generator.random and Generator.integers are those draws. Seeding reproduces the stream. That is pseudo-randomness, not a quantum measurement.
- Markov chain Monte Carlo, described on the Markov page, samples a hard distribution by running a classical chain whose next state depends only on the current one. Quantum hardware is not part of that description.
- Do not swap default_rng for a cryptographic or quantum source inside a simulation that only needed a seed. The page points cryptographic uses at the secrets module instead.
- A quantum algorithm would have to be a different sampler with its own interface. Nothing on the NumPy random page or the Markov page provides that interface.

**Sources:**

https://numpy.org/doc/stable/reference/random/index.html
https://en.wikipedia.org/wiki/Markov_chain
</details>

<details>
<summary>What role does probability play in reinforcement learning and decision making?</summary>

### What role does probability play in reinforcement learning and decision making?

**Definition:**

An action is chosen because of the probability of what happens next, not because the next state is known. When that probability depends only on the current state and the action, the process is Markov.

**How it works:**

- The Markov page's property is the usual modeling assumption: given the present, the past does not change the distribution of the future. The transition table is P(next | state, action).
- A return is an expectation of rewards along those transitions. A decision compares expectations. A single unlucky trajectory is not the value.
- Uncertainty in the transition is why one rollout is not enough. Monte Carlo estimates the expectation by many draws. numpy Generator supplies the classical draws. Markov chain Monte Carlo is the related sampler when the distribution is the hard object.
- If the next reward depends on a hidden inventory the state forgot, the Markov property fails and the expectation is conditioned on the wrong present.

**Sources:**

https://en.wikipedia.org/wiki/Markov_chain
https://numpy.org/doc/stable/reference/random/index.html
</details>

<details>
<summary>Describe how Monte Carlo simulations are used in machine learning for approximation of probabilities.</summary>

### Describe how Monte Carlo simulations are used in machine learning for approximation of probabilities.

**Definition:**

Monte Carlo replaces an integral or a huge sum by the average of many random draws. numpy.random.Generator produces those draws. Markov chain Monte Carlo is the version that draws by running a Markov chain when independent draws are hard.

**How it works:**

- default_rng().random(n) is uniform on [0, 1). integers and standard_normal cover other classical distributions. The average of a function of those draws estimates an expectation. The coin-toss running mean is the small case.
- The Markov page describes MCMC as simulation that samples a complex distribution by a chain whose next step depends only on the current state. The estimate is still an average along the chain, after the chain has moved.
- A seed reproduces the estimate. Without a seed the next run differs, because default_rng reads OS entropy. Report the number of draws with the estimate.
- The page says these generators are not for cryptography. A Monte Carlo error bar is a statistical statement about the draws, not a security claim.

**Sources:**

https://numpy.org/doc/stable/reference/random/index.html
https://en.wikipedia.org/wiki/Markov_chain
</details>

<details>
<summary>How do GANs (Generative Adversarial Networks) utilize probability theory?</summary>

### How do GANs (Generative Adversarial Networks) utilize probability theory?

**Definition:**

A generator defines a distribution by transforming noise into samples. A discriminator estimates the probability that a sample came from the data rather than from the generator. Training moves both.

**How it works:**

- The noise source in this numerical stack is a Generator. standard_normal draws the usual Gaussian noise. Those draws are classical and reproducible with a seed. The numpy page says they are not a cryptographic source.
- The discriminator's score is a probability. Cross-entropy, which scipy.stats.entropy relates to H(pk) + D(pk || qk), is the usual penalty for putting low probability on the true origin of a sample.
- A Gaussian mixture is the simpler generative model on the mixture page: it samples from fitted components and does not train an adversary. EM raises the likelihood directly. A GAN does not write that likelihood down.
- When the discriminator's probability stays near 1/2, the generator's samples are hard to tell from the data. That is a statement about two distributions, not about one point estimate.

**Sources:**

https://numpy.org/doc/stable/reference/random/index.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.entropy.html
https://scikit-learn.org/stable/modules/mixture.html
</details>

<details>
<summary>What are the probabilistic underpinnings of Active Learning and how might they be utilized in algorithm design?</summary>

### What are the probabilistic underpinnings of Active Learning and how might they be utilized in algorithm design?

**Definition:**

Active learning asks for the next label where the model is most uncertain. Uncertainty is a probability distribution over classes, not a distance.

**How it works:**

- LogisticRegression.predict_proba returns that distribution. For three or more classes it is a softmax. scipy.stats.entropy of one row is the Shannon uncertainty. A uniform row has the highest entropy. A row near 0 and 1 has entropy near 0.
- Label the high-entropy rows first. A confident wrong row is a different failure. Entropy does not see that the confident class is false. It only sees the spread.
- Naive Bayes also exposes a class distribution, and the page says not to trust the numeric probabilities. If you query by those values, check that the ranking of uncertainty matches held-out errors.
- A Gaussian mixture's responsibility vector is the same idea for clustering: ask a human about a point that sits between components, where no responsibility is near 1.

**Sources:**

https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html
https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.entropy.html
https://scikit-learn.org/stable/modules/naive_bayes.html
https://scikit-learn.org/stable/modules/mixture.html
</details>
