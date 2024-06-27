A Qablet timetable defines a financial contract using a sequence of payments, choices and conditions. A valuation model implemented with a Qablet parser can value any contract, as long as the contract can be described using a **timetable** such as this one -

```
          track        time op  quantity unit
        0    #1  03/31/2024  >       0.0  USD
        1    #1  03/31/2024  +   -2900.0  USD
        2    #1  03/31/2024  +       1.0  SPX
```


## Overview

 - Start with [Getting Started](quickstart.md)
 - For the model specific details see the **MODELS** section.
 - For viewing various stats see the **STATS** section.


## Other Resources

- [Qablet Contracts](https://qablet.github.io/qablet-contracts/) documents the timetable semantics, and a library of contract classes. 
- [Qablet Learning Path](https://github.com/qablet-academy/intro) is a set of Jupyter notebooks from simple to advanced uses of Qablet.
- [Qablet App](https://apps-dash.onrender.com/) is an interactive showcase of several Qablet contracts, with pricing and backtesting.
