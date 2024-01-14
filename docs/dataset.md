## Dataset

All models in the `qablet` package have a signature as follows:
```python
price, stats = model.price(timetable, dataset)
```
The arg timetable is a qablet timetable. The arg dataset is a dict with the following components

 - **BASE** String containing the name of the base asset, i.e. the currency in which the price is denominated.
 - **ASSETS** Dict containing forwards of all assets in the contract, including the base asset. See [Assets](assets.md) for more.
 - **{Model Family Name}** Dict containing parameters for the model family.
 - **{Model Name}** Dict containing parameters of the model. See the **MODELS** section for more.

### Example
```python
dataset = {
    "BASE": "USD",
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

