## Monte-Carlo Models

All Monte-Carlo Models include a common section (`MC`), and a model dependent section.
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
## finmc models

The `finmc` package contains Monte-Carlo implementations of many financial models derived from a common interface class.
You can price qablet contracts using `finmc` models by creating a model instance as follows.

```
from finmc.models.heston import HestonMC
from qablet.base.mc import MCPricer
model = MCPricer(HestonMC)
```

See a [complete example here.](../../quickstart/#example-2-heston-model)

See the documentattion of `finmc` models here -

- [Heston](https://finlib.github.io/finmc/models/heston/)
- [Hull-White](https://finlib.github.io/finmc/models/hullwhite/)
- [LocalVol](https://finlib.github.io/finmc/models/localvol/)

The dataset should follow the requirements of the corresponding finmc model. Additionally,
it should contain the `PRICING_TS` component for the datetime that we are calculating price as of.

```
dataset = {
    "BASE": "USD",
    "PRICING_TS": py_to_ts(pricing_dt).value,
    ...
```