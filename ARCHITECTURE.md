# Architecture

This document explains how std is structured internally. Read this if you want to add a new module, understand how imports resolve, or just go deeper than CONTRIBUTING.md covers.

---

## How modules work

Every module is a folder inside `src/`:

```
src/
├── math/
│   ├── main.rey
│   ├── trig.rey
│   └── README.md
├── fs/
│   ├── main.rey
│   ├── read.rey
│   ├── write.rey
│   └── README.md
└── io/
    ├── main.rey
    └── README.md
```

When a user writes:

```rey
import std::math
```

The compiler resolves this to `src/math/main.rey`. That file is the public surface of the module — everything marked `pub` in it is what the user gets.

Internal files like `trig.rey` or `read.rey` hold the actual implementations. They are not directly importable by users.

---

## main.rey is the contract

`main.rey` is the only file that matters to the outside world. Think of it as the module's public API. It should:

- Define or re-export every `pub` function in the module
- Import from internal files where needed
- Stay clean and readable — it's the first thing a new contributor sees

Internal files can be as messy or as split up as needed. `main.rey` should always be tidy.

---

## Adding a new module

1. Create a folder under `src/` with the module name
2. Add `main.rey` — start with at least one `pub func`
3. Add `README.md` — what the module does, what's implemented, what's missing
4. Add a folder under `tests/` with the same name
5. Write at least one test in `tests/<module>/`
6. Add the module to the table in the root `README.md`

That's the full checklist. Don't add a module without all five.

---

## Adding a new file to an existing module

If a module is getting large, split it into internal files:

1. Create the new file inside the module folder (e.g. `src/math/trig.rey`)
2. Move the relevant functions there
3. Import them back into `main.rey` if needed
4. The public API in `main.rey` should not change

Users should never notice an internal refactor.

---

## Tests

Tests live in `tests/<module>/` and mirror the structure of `src/<module>/`. One test file per function, named `<function>_test.rey`.

Tests are plain Rey files. Run them directly with the compiler:

```bash
rey tests/math/clamp_test.rey
```

There is no test runner yet. This will change as the tooling matures.

---

## What std is not

std is not a framework. It does not have opinions about how you structure your project. It provides primitives — the smallest useful building blocks — and gets out of the way.

If something feels too high-level or too opinionated for std, it probably belongs in a third-party package instead.