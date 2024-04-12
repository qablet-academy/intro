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

The model can be used as
```python
from qablet.heston.mc import HestonMCModel

dataset = {
    "BASE": ...
    "PRICING_TS": ...
    "ASSETS": ...
    "MC": ...
    "HESTON": ...
}
heston_model = HestonMCModel()
price, stats = model.price(timetable, dataset)
```

### Local Vol Model
`qablet.black_scholes.mc.LVMCModel`

In the Local Vol model the lognormal stock process \(X_t\) is given by,

$$
dX_t = (\mu - \frac{\sigma_t^2}{2}) dt + \sigma_t dW_s
$$

Where \(\sigma_t\) is a function of \(X_t\) and \(t\).

The model specific component in the dataset (`LV`) is a dict with two parameters `ASSET` and `VOL`.

#### Fixed Vol
`VOL` can be a float as shown below, in which case it reduces to the Black-Scholes Model.

```python
"LV": {
    "ASSET": "SPX",
    "VOL": 0.015
}
```
#### Vol Function
`VOL` can be a function as below

```python
def volfn(points):
    # t is float, x_vec is a np array
    (t, x_vec) = points

    at = 5.0 * t + .01
    atm = 0.04 + 0.01 * np.exp(-at)
    skew = -1.5 * (1 - np.exp(-at)) / at
    return np.sqrt(np.maximum(0.001, atm + x_vec * skew))


"LV": {
    "ASSET": "SPX",
    "VOL": volfn
}
```

#### Vol Interpolator
`VOL` can be an interpolator as below

```python
from scipy.interpolate import RegularGridInterpolator

times = [0.01, 0.2, 1.0]
strikes = [-5.0, -0.5, -0.1, 0.0, 0.1, 0.5, 5.0]
vols = np.array([
    [2.713, 0.884, 0.442, 0.222, 0.032, 0.032, 0.032],
    [2.187, 0.719, 0.372, 0.209, 0.032, 0.032, 0.032],
    [1.237, 0.435, 0.264, 0.200, 0.101, 0.032, 0.032]
])
volinterp = RegularGridInterpolator(
    (times, strikes), vols, fill_value=None, bounds_error=False
)

"LV": {
    "ASSET": "SPX",
    "VOL": volinterp
}
```

The model can be used as
```python
from qablet.black_scholes.mc import LVMCModel

dataset = {
    "BASE": ...
    "PRICING_TS": ...
    "ASSETS": ...
    "MC": ...
    "LV": ...
}
heston_model = LVMCModel()
price, stats = model.price(timetable, dataset)
```