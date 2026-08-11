# Frequentist and Bayesian Treatment Effect Analysis

## Topic

**Frequentist vs Bayesian Inference in a Marketing Experiment**

---

## Overview

This project compares **frequentist and Bayesian approaches** for estimating the **causal effect of a personalized marketing email on customer purchase behavior**.

Customer-level data are simulated using a **logistic data-generating process (DGP)** that includes:

- Randomized treatment assignment
- Customer engagement
- Income group
- Binary purchase outcome

Because the **true treatment effect is known** in the simulation, the project evaluates how accurately each statistical approach recovers the underlying effect and how **estimation uncertainty changes as the sample size increases**.

The analysis also explores several practical challenges commonly encountered in real-world experimentation, including:

- **Repeated significance testing**
- **Treatment-effect heterogeneity**
- **Prior sensitivity**
- **Sample-size sensitivity**
- The difference between **statistical significance and business importance**

---

## Methodology

### Frequentist Analysis

The frequentist analysis includes:

- Treatment and control **purchase-rate comparisons**
- **Average Treatment Effect (ATE)** estimation
- Standard errors and **95% confidence intervals**
- Hypothesis testing
- **OLS / Linear Probability Models**
- **Logistic regression**
- Sample-size comparisons
- **Sequential testing** and false-positive inflation
- Interaction models
- Subgroup treatment-effect analysis

The general logistic model is:

```math
P(Y_i = 1) = \text{logit}^{-1}(\alpha + \tau D_i + \beta X_i + \gamma Z_i)
```

where:

- $Y_i$ = binary purchase outcome
- $D_i$ = treatment assignment (personalized email vs. control)
- $X_i$ = customer engagement
- $Z_i$ = customer income group
- $\tau$ = treatment coefficient of primary interest
- $\alpha$ = baseline intercept
- $\beta$ and $\gamma$ = coefficients for customer characteristics
- $\sigma(\cdot)$ = logistic function converting the linear predictor into a purchase probability

Because the outcome is binary, the logistic model estimates how treatment and customer characteristics affect the **probability of purchase**.

---

### Bayesian Analysis

The Bayesian analysis uses **PyMC** and **NUTS (No-U-Turn Sampler)** to estimate posterior distributions for the logistic-regression parameters.

The analysis includes:

- Posterior treatment-effect estimation
- **Bayesian credible intervals**
- Probability that the treatment effect is positive
- Bayesian ATE estimation on the **purchase-probability scale**
- Probability that the ATE exceeds a practical business threshold
- **Prior-sensitivity analysis**
- **Sample-size sensitivity analysis**
- MCMC convergence diagnostics
- Posterior uncertainty analysis

One important Bayesian quantity is:

$$
P(\tau > 0 \mid \text{data})
$$

which represents the posterior probability that the treatment coefficient is positive.

The analysis also calculates:

$$
P(ATE > 0.01 \mid \text{data})
$$

which represents the posterior probability that the treatment increases the purchase rate by **more than one percentage point**.

---

## Main Findings

Both **frequentist logistic regression** and **Bayesian logistic regression** recover the true treatment coefficient when the sample size becomes sufficiently large.

As the sample size increases:

- Treatment-effect estimates become more stable
- Standard errors decrease
- Confidence intervals become narrower
- Bayesian credible intervals become narrower
- Posterior uncertainty decreases
- Evidence that the treatment effect is positive becomes stronger

With smaller samples, treatment-effect estimates fluctuate more substantially, and **Bayesian conclusions can be more sensitive to the selected prior distribution**.

### Sequential Testing

The analysis also demonstrates an important problem with **sequential hypothesis testing**.

Repeatedly checking p-values and stopping an experiment as soon as:

$$
p < 0.05
$$

can increase the probability of obtaining a **false-positive conclusion**, even when the true treatment effect is zero.

This demonstrates why organizations prefer to define **sample sizes and stopping rules before running an experiment** rather than continuously checking significance and stopping whenever a desired result appears.

---

### Treatment-Effect Heterogeneity

Interaction models indicate that the personalized marketing email is **more effective among highly engaged customers**.

Subgroup analysis provides an intuitive way to communicate treatment-effect differences across customer segments.

The **pooled interaction model**, however, provides a stronger formal statistical test of whether the treatment effect actually differs according to customer engagement.

These findings demonstrate that an intervention can have a positive overall effect while still producing **different effects across customer groups**.

This is particularly important for businesses because the optimal strategy may not be to send the same treatment to every customer.

---

## Tools and Libraries

The project was developed in **Python** using the following tools and libraries:

- **NumPy** — numerical operations, random sampling, and probability calculations
- **Pandas** — creating, transforming, and summarizing customer-level datasets
- **SciPy** — frequentist hypothesis testing
- **Statsmodels** — OLS, linear probability models, and logistic regression
- **PyMC** — Bayesian logistic regression and posterior sampling
- **ArviZ** — posterior summaries, credible intervals, effective sample size, and MCMC convergence diagnostics
- **Jupyter Notebook** — running the analysis, documenting results, and presenting code interactively

---

## Business Implications

This project demonstrates how statistical experimentation can be translated into **practical business decisions**.

A traditional frequentist result may report that a treatment is statistically significant:

> **"We reject the null hypothesis at the 5% significance level."**

While statistically valid, this statement can sometimes be difficult for non-technical stakeholders to interpret directly.

A Bayesian analysis can instead provide a probability statement such as:

$$
P(\tau > 0 \mid \text{data}) = 97\%
$$

which can be interpreted as:

> **"Given the model, prior, and observed data, there is a 97% posterior probability that the treatment effect is positive."**

This type of statement directly addresses a common business question:

> **"How likely is it that the treatment actually works?"**

Bayesian analysis can also evaluate whether the treatment effect is large enough to be **practically valuable**, rather than simply statistically different from zero.

For example:

$$
P(ATE > 0.01 \mid \text{data})
$$

represents the posterior probability that the personalized marketing email increases the customer purchase rate by **more than one percentage point**.

This can changes the decision from simply asking:

> **"Is the effect statistically significant?"**

to asking:

> **"How likely is the treatment to create a meaningful improvement for the business?"**

These probability-based statements can be easier for managers and business stakeholders to interpret and can directly support decisions about whether to:

- **Launch** an intervention
- **Continue** an experiment
- **Modify** the treatment strategy
- **Target specific customer segments**
- **Stop** an intervention that is unlikely to generate meaningful value

The result of the finding also emphasize that organizations should:

1. **Define sample sizes and testing plans before launching experiments**
2. **Avoid stopping experiments immediately after observing statistical significance**
3. Consider both **statistical significance and practical business importance**
4. Examine whether treatment effects differ across **customer segments**
5. Evaluate uncertainty before making business decisions
6. Communicate statistical results in a way that supports **business decision-making**

---

## Key Takeaway

The project highlights that **frequentist and Bayesian approaches answer related but different statistical questions**.

**Frequentist inference** focuses primarily on:

- Hypothesis testing
- P-values
- Confidence intervals
- Long-run statistical properties

**Bayesian inference** focuses on:

- Posterior distributions
- Credible intervals
- Direct probability statements about parameters
- Prior information
- Probability of practically meaningful treatment effects

Both approaches can provide valuable information, but they communicate uncertainty differently.

Together, these methods demonstrate how rigorous experimentation can move beyond simply asking:

> **"Is the treatment statistically significant?"**

toward the more decision-relevant question:

> **"How likely is the treatment to create a meaningful improvement for the business?"**
