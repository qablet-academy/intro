## Finite Difference Models

All Finite-Difference Models (one factor) include a common section (`FD`), and a model dependent section. The common section has the following parameters.

- **TIMESTEP**: The incremental timestep of simulation (in years).
- **MAX_X**: Distance of the upper (or lower) boundary of the model grid from the center.
- **N_X**: The number of levels from the center, in each direction.

e.g.
```python
"FD": {
    "TIMESTEP": 1 / 250,
    "MAX_X": 0.1,
    "N_X": 250,
},
```

### Black-Scholes Model
`qablet.black_scholes.fd.BSFDModel`

In the Heston model the lognormal stock process \(X_t\) is given by,

$$
dX_t = (\mu_t - \frac{\sigma^2}{2})dt + \sigma dW_t
$$

The model specific component in the dataset (`BS`) is a dict with `VOL` (\(\sigma\)), and the name of the asset:

e.g.
```python
"BS": {
    "ASSET": "SPX",
    "VOL": 0.3,
}
```

### Hull-White Model
`qablet.hullwhite.fd.HWFDModel`

In the Hull White model, the short-rate follows the following process.
$$
dr_t = [\theta_t - a r_t]dt + \sigma dW_t
$$

The model specific component in the dataset (`HW`) is a dict with the parameters:

* \(a\), the mean reversion rate (MEANREV).
* \(\sigma\), the volatility (VOL).

e.g.
```python
"HW": {
    "MEANREV": 0.1,
    "VOL": 0.03,
}
```