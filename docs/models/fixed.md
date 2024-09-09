## Fixed Model

The fixed model requires no model specific parameters.

```
dataset = {
    "BASE": "USD",
    "PRICING_TS": py_to_ts(datetime(2023, 12, 31)).value,
    "ASSETS": {"USD": discount_data},
}
```

See a [complete example here.](../../quickstart/#example-1-fixed-model)
