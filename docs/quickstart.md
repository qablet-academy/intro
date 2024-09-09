You will need the following packages.

- `qablet-contracts` contains utilities to create qablet timetables for financial contracts.
- `qablet-basic` contains a suite of models to evaluate qablet timetables.

```
pip install qablet-contracts
pip install qablet-basic
```

### Example 1: Fixed Model

In this example we create a zero coupon bond, and price it using a deterministic/fixed model.

```
import numpy as np
from pyarrow import RecordBatch as rb
from datetime import datetime
from qablet_contracts.timetable import TS_EVENT_SCHEMA, py_to_ts
from qablet.base.fixed import FixedModel

# Create Timetable
events = [
    {
        "track": "",
        "time": datetime(2024, 12, 31),
        "op": "+",
        "quantity": 100.0,
        "unit": "USD",
    },
]
timetable = {
    "events": rb.from_pylist(events, schema=TS_EVENT_SCHEMA)
}

# Create Dataset for FixedModel
discount_data = ("ZERO_RATES", np.array([[5.0, 0.04]])) # 5yr : 4%
dataset = {
    "BASE": "USD",
    "PRICING_TS": py_to_ts(datetime(2023, 12, 31)).value,
    "ASSETS": {"USD": discount_data},
}

# Calculate Price with FixedModel
model = FixedModel()
price, _ = model.price(timetable, dataset)
print(f"price: {price:11.6f}")
```

### Example 2: Heston model

In this example we price an vanilla call option using Heston model from the `finmc` package.

```
import numpy as np
from datetime import datetime
from qablet_contracts.eq.vanilla import Option
from qablet_contracts.timetable import py_to_ts
from finmc.models.heston import HestonMC
from qablet.base.mc import MCPricer

# Create option contract using qablet_contracts
pricing_dt = datetime(2024, 3, 15)
maturity = datetime(2024, 7, 31)
spot = 171.17
contract = Option(
    "USD",
    "AAPL",
    strike=spot,
    maturity=maturity,
    is_call=True,
)

contract.print_events()

timetable = contract.timetable()

# Create dataset for Heston model
discount_data = ("ZERO_RATES", np.array([[5.0, 0.05]]))
fwd_data = ("FORWARDS", np.array([[0.0, spot], [1.0, spot * 1.03]]))
dataset = {
    "BASE": "USD",
    "PRICING_TS": py_to_ts(pricing_dt).value,
    "ASSETS": {"USD": discount_data, "AAPL": fwd_data},
    "MC": {
        "PATHS": 100_000,
        "TIMESTEP": 1 / 250,
        "SEED": 1,
    },
    "HESTON": {
        "ASSET": "AAPL",
        "INITIAL_VAR": 0.015,
        "LONG_VAR": 0.052,
        "VOL_OF_VOL": 0.88,
        "MEANREV": 2.78,
        "CORRELATION": -0.85,
    },
}

# Price
model = MCPricer(HestonMC)
price, _ = model.price(timetable, dataset)
print(f"price: {price:11.6f}")
```
### Next Step

Dive into the
[Qablet Learning Path](https://github.com/qablet-academy/intro/blob/main/notebooks/1_1_fixed_bond.ipynb) next.
It is a set of Jupyter notebooks that will walk you through simple to advanced uses of Qablet.

## Other Resources
- See [Qablet Contracts](https://qablet.github.io/qablet-contracts/) for the timetable semantics, and a library of common financial contracts.
- See [Dataset API](dataset.md) to construct a dataset from your market environment.
- See [Finite Difference/PDE](models/fd.md) and [Monte Carlo](models/mc.md) models in the `qablet-basic` package.
- Try [Qablet App](https://apps-dash.onrender.com/) - an interactive showcase of several Qablet contracts, for pricing and backtesting.
