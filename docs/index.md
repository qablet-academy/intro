## Introduction

[qablet-basic](https://pypi.org/project/qablet-basic/) contains a suite of models to evaluate qablet timetables. It includes -

- [Finite Difference/PDE models](models/fd.md) such as Hull-White and Black-Scholes
- [Fixed model](models/fixed.md)
- A [Monte Carlo](models/mc.md) pricer to use any model defined using the [finmc](https://finlib.github.io/finmc/) interface.


## What is a Qablet Timetable?
A Qablet timetable defines a financial contract using a sequence of payments, choices and conditions. A Qablet model can value any contract, as long as the contract can be described using a **timetable** such as this one -

```
          track        time op  quantity unit
        0    #1  03/31/2024  >       0.0  USD
        1    #1  03/31/2024  +   -2900.0  USD
        2    #1  03/31/2024  +       1.0  SPX
```

See [Qablet Contracts](https://qablet.github.io/qablet-contracts/) for the timetable semantics, and a library of common financial contracts.


## Getting Started

install from pip
```
pip install qablet-basic
```

Start with [Example 1: Hello World](quickstart.md)

