## Monte-Carlo Models

All Monte-Carlo Models include a common section (`MC`), and a model dependent section.

### Common Section

The common section has the following parameters.

- **PATHS**: The number of Monte-Carlo paths.
- **TIMESTEP**: The incremental timestep of simulation (in years). 
- **SEED**: The seed for the random number generator.

e.g.
```python
"MC": {
    "PATHS": 100_000,
    "TIMESTEP": 1 / 250,
    "SEED": 1,
},
```

### Heston Model
`qablet.heston.mc.HestonMCModel`

In the Heston model  the lognormal stock process \(X_t\) is given by,

$$
dX_t = (\mu - \frac{\nu_t}{2}) dt + \sqrt \nu_t dW_s
$$

and the variance follows the process
$$
d \nu_t = \kappa (\theta - \nu_t) dt + \xi \sqrt \nu_tdW_t
$$

where \(dW_s\) and \(dW_t\) are Wiener processes with correlation \(\rho\).

The model specific component in the dataset (`HESTON`) is a dict with five parameters, and the name of the asset:

* \(\nu_0\), the initial variance (INITIAL_VAR).
* \(\theta\), the long variance (LONG_VAR).
* \(\rho\), the correlation (CORRELATION).
* \(\kappa\), the mean reversion rate (MEANREV)).
* \(\xi\), the volatility of the volatility (VOL_OF_VAR).

e.g.
```python
"HESTON": {
    "ASSET": "SPX",
    "INITIAL_VAR": 0.015,
    "LONG_VAR": 0.052,
    "VOL_OF_VAR": 0.88,
    "MEANREV": 2.78,
    "CORRELATION": -0.85,
}
```