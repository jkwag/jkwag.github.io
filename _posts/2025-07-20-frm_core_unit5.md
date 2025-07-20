---
title: "FRM Module 5 핵심 정리"
date: 2025-07-20
categories: [FRM]
tags: []
toc: true
math: true
toc_label: "Table of contents"
toc_icon: "cog"
---

## Diversification
- **Rational Investors** seek to **maximize return per unit of risk** -> absent a risk-free asset,
they will hold a **portfolio on the efficient frontier**
- To reduce total risk, investors diversify across multiple investments
- A sufficiently large porfolio will have **eliminated company-specific(idiosyncratic)
risk** and will only be **exposed to market risk**

## CAPM

- Expectations on CAPM(Capital Asset Pricing Model)
  - Expected return **only depends on beta** because **company-specific risk
can be diversified away**
  - Expected return is a **linear function of beta**
- The equation:

$$ E(R_{i}) = R_{F} + [E(R_{M}) - R_{F}]\beta_{i}$$

- The beta of the market is equal to 1, and the slope of the **security makret line(SML)** is
- equal to the **market risk premium(MRP)**. The SML is the graphical depiction of the CAPM

## Assumptions on CAPM

- Information is freely available
- There are no taxes and commissions
- Fractional investments are possible
- Market participants can borrow and lend at the risk-free rate
- Individual investors cannot affect market prices
- Investors have the same forecasts of expected returns, variances, and covariances

## Capital Market Line

- The CML **linearly combines the risk-free asset with the tangency portfolio of the efficient frontier**
- Given the assumption of homogeneous expectations, the tangency porfolio becomes **the market portfolio**
- All investors are assumed to **hold some combination of the risk-free asset and the market portfolio**
- Equation:

$$ E(R_{p}) = R_{F} + \frac{[E(R_{M}) - R{F}]}{\sigma_{M}}\sigma_{P}$$

- The MRP is the return of the market in excess of the risk-free rate

## Derivation of Beta

- Beta can be estimated as the **slope from a linear regression of stock returns against market returns**
- **Sensitivity of stock returns to market movements**
- Equation:

$$ B_{i} = \frac{covariance of Asset i's return with the market return}{variance of the market return} = \fraction{Cov_{i,M}}{\sigma^{2}_{M}} = p_{i,M}\times \frac{\sigma_{i}}{\sigma_{M}}
