You will need the following packages.

- `qablet-contracts` contains utilities to create qablet timetables for financial contracts.
- `qablet-basic` contains a suite of models to evaluate qablet timetables.

```
pip install qablet-contracts
pip install qablet-basic
```

### Hello World

This is a simple example of creating a zero coupon bond, and pricing it using a deterministic model.

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
discount_data = ("ZERO_RATES", np.array([[0.0, 0.04], [5.0, 0.04]])) # 4% from 0-5 years
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

### Next Steps
- See [Qablet Contracts](https://qablet.github.io/qablet-contracts/) to create timetables with payments, options, triggers and path dependent features.
- See [Dataset API](dataset.md) to construct a dataset from your market data.
- See [Finite Difference/PDE](models/fd.md) and [Monte Carlo](models/mc.md) models that comes with the `qablet-basic` package.

We recommend that you bookmark the above references, and dive into the
[Qablet Learning Path](https://github.com/qablet-academy/intro) next.
It is a set of Jupyter notebooks that will walk you through simple to advanced uses of Qablet.
