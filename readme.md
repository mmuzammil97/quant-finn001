# Quant Finn: Option Pricing Basics

This repository contains a notebook-based introduction to option pricing with:
- European call payoff visualization
- 2-step binomial pricing
- Multi-step binomial pricing (European/American)
- Monte Carlo pricing under geometric Brownian motion
- Black-Scholes-Merton (BSM) analytical pricing for a European call

Primary notebook: `binomial-trees-basics.ipynb`

## Theory Overview

### 1. Call Payoff at Maturity
For a European call option with strike $K$, terminal stock price $S_T$, and maturity $T$:

$$
\text{Payoff}_{\text{call}}(T) = \max(S_T - K, 0)
$$

This is the intrinsic value at expiry and is the terminal condition used by tree and Monte Carlo methods.

### 2. Risk-Neutral Valuation
Under no-arbitrage, option value is the discounted risk-neutral expectation of payoff:

$$
V_0 = e^{-rT}\mathbb{E}^{\mathbb{Q}}[\text{Payoff}(S_T)]
$$

where:
- $r$: continuously compounded risk-free rate
- $\mathbb{Q}$: risk-neutral measure

This principle unifies binomial trees, Monte Carlo, and BSM.

### 3. Binomial Model
Over a single time step $\Delta t$, stock moves:

$$
S \to Su \quad \text{or} \quad Sd
$$

with risk-neutral probability:

$$
p = \frac{e^{r\Delta t} - d}{u - d}
$$

The option is priced by backward induction:
1. Compute payoff at terminal nodes.
2. Discount expected continuation value one step at a time.
3. For American options, compare continuation vs immediate exercise at each node.

As the number of steps increases, European binomial prices converge toward BSM prices under standard parameterization.

### 4. Monte Carlo Simulation
Under BSM assumptions, the terminal stock price follows:

$$
S_T = S_0 \exp\left(\left(r - \frac{1}{2}\sigma^2\right)T + \sigma\sqrt{T}\,Z\right), \quad Z\sim\mathcal{N}(0,1)
$$

Then:

$$
V_0 \approx e^{-rT}\frac{1}{N}\sum_{i=1}^{N}\text{Payoff}(S_T^{(i)})
$$

Monte Carlo is flexible for complex payoffs, with error decreasing approximately as $1/\sqrt{N}$.

### 5. Black-Scholes-Merton (Analytical European Call)
For continuous dividend yield $q$, the closed-form European call price is:

$$
C = S_0 e^{-qT}N(d_1) - K e^{-rT}N(d_2)
$$

$$
d_1 = \frac{\ln(S_0/K) + (r - q + \tfrac{1}{2}\sigma^2)T}{\sigma\sqrt{T}}, \qquad
d_2 = d_1 - \sigma\sqrt{T}
$$

where $N(\cdot)$ is the standard normal CDF.

## Assumptions (BSM Framework)
- Frictionless markets (no taxes/transaction costs)
- Continuous trading and divisibility
- Constant $r$, $\sigma$, and $q$
- No arbitrage
- Stock follows geometric Brownian motion
- European exercise (for the closed-form call above)

## What the Notebook Demonstrates
- How payoff shape drives pricing at maturity
- How replication/no-arbitrage appears in discrete-time trees
- How simulation approximates risk-neutral expectations
- How numerical methods compare to the analytical BSM benchmark

## Setup
```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## Run
Open and run:
- `binomial-trees-basics.ipynb`
