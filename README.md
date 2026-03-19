# std

The official standard library for the Rey programming language.

---

## What is this?

std is the built-in set of modules that ship with Rey. Filesystem access, math, I/O, and more — all written in Rey, for Rey.

When you write `import std::math`, this is what you're pulling from.

---

## Why does it exist?

Rey's thesis is power without complexity. std is where that plays out in practice — common, well-tested building blocks so you're never reaching for a third-party package to do something basic.

---

## Modules

| Module | What it does |
|--------|--------------|
| `std::math` | Math utilities — rounding, clamping, trig, random |
| `std::fs` | Read and write files |
| `std::io` | Input and output beyond `println` |

More modules will be added as the language grows.

---

## Contributing

std is built by the community. If a function is missing, you can add it.

Read [CONTRIBUTING.md](./CONTRIBUTING.md) to get started.