
All models in the `qablet` package have a signature as follows:
The arg timetable is a [qablet timetable](https://qablet.github.io/qablet-contracts/).

```python
price, stats = model.price(timetable, dataset)
```

## Dataset

The arg dataset is a dict with the following components

 - **BASE** String containing the name of the base asset, i.e. the currency in which the price is denominated.
 - **PRICING_TS** The timestamp (milliseconds) that we will price the contract as of.
 - **ASSETS** Dict containing forwards of all assets in the contract, including the base asset. See [Assets](assets.md) for more.
 - **{Model Family Name}** Dict containing parameters for the model family such as **MC** or **FD**.
 - **{Model Name}** Dict containing parameters of the model. See the **MODELS** section for more.

### Example
```python
import numpy as np
from datetime import datetime

times = np.array([0.0, 1.0, 2.0, 5.0])  # in years
rates = np.array([0.04, 0.04, 0.045, 0.05])  # i.e. 4%, etc
fwds = np.array([100.0, 101.0, 102.0, 104.0])
discount_data = ("ZERO_RATES", np.column_stack((times, rates)))
fwd_data = ("FORWARDS", np.column_stack((times, fwds)))

dataset = {
    "BASE": "USD",
    "PRICING_TS": datetime(2023, 12, 31),
    "ASSETS": {"USD": discount_data, "SPX": fwd_data},
    "MC": {
        "PATHS": 100_000,
        "TIMESTEP": 1 / 250,
        "SEED": 1,
    },
    "HESTON": {
        "ASSET": "SPX",
        "INITIAL_VAR": 0.015,
        "LONG_VAR": 0.052,
        "VOL_OF_VAR": 0.88,
        "MEANREV": 2.78,
        "CORRELATION": -0.85,
    }
}
```

