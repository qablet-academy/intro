## Fixed Model

The qablet package includes a Fixed Model that calculates price using deterministic cashflows and discount curve.
The dataset for a fixed model requires no model specific parameters, just the common one [described here](../dataset.md)

```
dataset = {
    "BASE": "USD",
    "PRICING_TS": datetime(2023, 12, 31),
    "ASSETS": {"USD": discount_data},
}
```

See a [complete example here.](../quickstart.md#example-1-fixed-model)
