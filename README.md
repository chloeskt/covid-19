# Project SMC Coronavirus 

This project was done for the course `Simulation et Monte Carlo` at ENSAE 2019-2020 by Chloé Sekkat, Ridouane Ghermi and Marc-Antoine Samson.

Notebook 1-0 presents a simple deterministic implementation of the advanced SEIR model developed by the authors of 
https://www.medrxiv.org/content/10.1101/2020.01.31.20019901v2. It focuses only on Wuhan's population and the parameters chosen are derived from the estimations of the former paper. 

Notebook 2-0 introduces stochastic methods in order to simulate according to the advanced SEIR model. `gamma` and `sigma` are believed to follow exponential laws. We aimed at estimating both `prevalence` (i.e. the number of symptomatic cases each day) and `incidence` (i.e. the number of new symptomatic cases each day). \
Furthermore, in the paper `beta` (the transmission rate) is believed to follow a geometric Brownian motion, which we implemented using RQMC with Sobol sequence. The convergence rate is better and the approximation error smaller with the latter. \
Then we decided to apply this model to real French data in order to estimate both the `R0` before and after containment measures and the `prevalence` and the `incidence`. In between 2020-02-26 & 2020-03-25, we estimate a `R0` of 1.65 while after the containment measures where taken (2020-03-18), we approximate it to be aroung 1.07. 

Notebook 3-0 aims at getting more accurate estimations by integrating real data on the number of cases reported day after day during the first month and the number of deaths. This new data can be used through approximate bayesian computation (ABC). As we know, the most basic form of ABC is the ABC rejection algorithm. But we also know tha there is a huge tradeoff between the precision and the computation time, both depending on the threshold and the search space (prior). \
We solve this problem with ABC-SMC (approximate bayesian computation - sequential monte carlo). We believe that it can also be solved by using ABC-MCMC (Metropolis-Hastings). But we focus on the former method. \
The idea is very intuitive: instead of sampling many particles from the prior distribution until with accept enough of them with a given threshold, we will proceed sequentially with less and less coarser thresholds. At each step, we'll perform an ABC rejection algorithm but instead of using the prior distribution, we will use an intermediate posterior distribution estimated from the previous time step. By using diminishing threshold at each time step, we'll refine the posterior distribution. It solves both the precision and the computation time problems because the search space is shrinking throughout time as well as the threshold. \
Again we decided to apply this method on real French data. Our model gives a nice prediction of the number of confirmed cases in between 2020-02-26 and 2020-04-01 (roughly before the containment measures could have a real impact). \
Hereafter we use our ABC-SMC model in order to estimate the following parameters: 
- `beta`: the transmission rate 
- `sigma`: 1 / the incubation period 
- `gamma`: the rate of isolation
- `kappa`: the rate of reporting 

Notebook 4-0 tries to bring to light trends from the exponential growth of the epidemic. To do that, there are 3 key ideas :
- logarithmic scale (base 10)
- plot number of new cases (bell shape) instead of total number of cases (logistic scale)
- don't plot against time because the number of new cases depends on the current number of cases

But we also need to remember some caveats regarding this plot:
- time not shown
- confirmed cases $\neq$ infections
- spread is slower because testing is increasing
- pessimistic graph because trend is calculated on a week scale
