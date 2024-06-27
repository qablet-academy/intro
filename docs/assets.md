## Assets

The `ASSETS` component of the dataset describes the forwards of each asset that would be required to value a contract. This includes

- the base asset (currency) in which the contract is valued
- any asset (currency, equity, commodity) received by the contract
- any asset value used in a snapper, or in a model

In the present version the following schemas are supported to describe the forwards. This list will be extended in coming versions.

### Zero Rates

You can describe the base asset using a two-column (N X 2) numpy array, where the first column is time, and the second represents **term zero rates**, e.g.

```
[[0.   0.04]
 [1.   0.04]
 [5.   0.05]]
```

It can be created like

```python
discount_data = ("ZERO_RATES", np.array([[0.0, 0.04], [1.0, .04], [5.0, 0.05]]))
dataset = {
    "BASE": "USD",
    "PRICING_TS": ..
    "ASSETS": {"USD": discount_data},
}
```

Or alternatively, using `np.column_stack` from two arrays

```python
times = np.array([0.0, 1.0, 5.0])
rates = np.array([0.04, 0.04, 0.05])
discount_data = ("ZERO_RATES", np.column_stack((times, rates)))
```

### Forwards

You can describe any asset using a two-column (N X 2) numpy array, where the first column is time, and the second represents forwards, e.g.

```python
spot = 2900
div_rate = 0.01
times = np.array([0.0, 1.0, 2.0, 5.0])
rates = np.array([0.04, 0.04, 0.045, 0.05])
fwds = spot * np.exp((rates - div_rate) * times)
fwd_data = ("FORWARDS", np.column_stack((times, fwds)))

dataset = {
    "BASE": "USD",
    "PRICING_TS": ..
    "ASSETS": {"SPX": fwd_data},
}
```