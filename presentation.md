---
marp: true
theme: gaia
paginate: true
size: 16:9

---

<style>
:root {
--color-background: #fff;
--color-foreground: #333;
--color-highlight: #f96;
--color-dimmed: #888;
}

.row {
    display: flex;
}

.img-2row {
    max-width: 35%;
    padding: 0;
    margin: 0 auto;
}


</style>

# MxlPy & MxlBricks workshop

<p class="row">
    <img src="https://raw.githubusercontent.com/Computational-Biology-Aachen/MxlPy/refs/heads/main/docs/assets/logo-diagram.png" class="img-2row"
    alt='mxlpy-logo'>
    <img src="https://raw.githubusercontent.com/Computational-Biology-Aachen/mxl-bricks/refs/heads/main/docs/assets/logo.png" class="img-2row"
    alt='mxlbricks-logo'>
</p>

---

# All things derived

---

# Parameters

```python
(
    Model()
    .add_parameter("p1", 1.0)
    .add_derived("d1", fns.twice, args=["p1"])  # derive from parameter p1
    .get_dependent()
)
```

---

# Variables

```python
(
    Model()
    .add_variable("v1", 1.0)
    .add_derived("d1", fns.twice, args=["v1"])  # derive from variable v1
    .get_dependent()
)
```

---

# Derive from derived

```python
(
    Model()
    .add_parameter("p1", 1.0)
    .add_derived("d1", fns.twice, args=["p1"])
    .add_derived("d2", fns.twice, args=["d1"])  # derive from derived d1
    .get_dependent()
)
```

---

# Rates

```python
(
    Model()
    .add_variable("v1", 1.0)
    .add_reaction("r1", fns.twice, args=["v1"], stoichiometry={"v1": -1})
    .add_derived("d1", fns.twice, args=["r1"])  # derived from rate of r1
    .add_reaction("r2", fns.twice, args=["d1"], stoichiometry={"v1": -1})  # use d1!
    .get_dependent()
)
```

---

# Stoichiometries

```python
(
    Model()
    .add_parameter("p1", 1.0)
    .add_variable("v1", 1.0)
    .add_reaction(
        "r1",
        fns.twice,
        args=["v1"],
        stoichiometry={"v1": Derived(fn=fns.twice, args=["p1"])},
    )
    .get_stoichiometries()
)
```

---

# Initial conditions

> Note: this just derives the value **once**.
> This is **not** the same as a derived variable

```python
(
    Model()
    .add_variables(
        {
            "v1": 1.0,
            "v2": Derived(fn=fns.twice, args=["v1"]),  # derive initial condition
        }
    )
    .get_initial_conditions()
)
```
