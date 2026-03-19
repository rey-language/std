# Contributing to Rey Standard Library

Welcome. Whether this is your first open source contribution or your hundredth — this guide has everything you need.

---

## Table of Contents
1. [Who can contribute?](#who-can-contribute)
2. [Dev environment setup](#dev-environment-setup)
3. [How modules are structured](#how-modules-are-structured)
4. [Adding a new function](#adding-a-new-function)
5. [Code style and conventions](#code-style-and-conventions)
6. [Writing tests](#writing-tests)
7. [Opening a PR](#opening-a-pr)
8. [Code of Conduct](#code-of-conduct)

---

## Who can contribute?

Anyone. Seriously.

If you know what a function is, you can contribute to std. You don't need to know Rust, you don't need to understand compiler internals. You just need to write Rey code and follow this guide.

Good first contributions:
- Adding a missing math function (e.g. `math.clamp`, `math.lerp`)
- Improving a module README
- Writing more tests for existing functions
- Fixing a bug in an existing function

---

## Dev environment setup

### 1. Download the Rey compiler

Go to [github.com/rey-language/rey/releases](https://github.com/rey-language/rey/releases) and download the latest release for your platform:
- **macOS (ARM):** `rey-v0-macos-arm64`
- **Windows:** `rey-v0-windows-x64.exe`

### 2. Install and name it `rey`

**macOS (ARM)**

```bash
mv ~/Downloads/rey-v0-macos-arm64 /usr/local/bin/rey
chmod +x /usr/local/bin/rey
rey --version
```

**Windows**

```powershell
mkdir C:\rey
move "$env:USERPROFILE\Downloads\rey-v0-windows-x64.exe" C:\rey\rey.exe
[Environment]::SetEnvironmentVariable("Path", $env:Path + ";C:\rey", "User")
# Restart your terminal, then verify
rey --version
```

### 3. Clone the repo

```bash
git clone https://github.com/YOUR_USERNAME/std.git
cd std
```

That's it. No build step, no package install. Rey files are just files.

---

## How modules are structured

Every module lives in `src/<module>/`:

```
src/
└── math/
    ├── main.rey     ← entry point, wires everything together
    ├── trig.rey     ← implementation file
    ├── stats.rey    ← implementation file
    └── README.md    ← module-level docs for contributors
```

`main.rey` is the root of every module. It's what gets resolved when a user imports:

```rey
import std::math
```

They get everything marked `pub` across the module, with `main.rey` as the entry point. Internal files like `trig.rey` hold the actual implementations.

Each module has a `README.md` that explains what's implemented, what's missing, and how to add something new to that specific module. Always read it before touching the module.

---

## Adding a new function

Let's say you want to add `math.clamp(value, min, max)`.

**1. Check it doesn't already exist**

macOS:
```bash
grep -r "clamp" src/math/
```

Windows:
```powershell
Select-String -Path "src\math\*" -Pattern "clamp"
```

**2. Implement it in the right file**

Simple utility? Add it to `main.rey`. Part of a clear subcategory? Add it to the relevant file or create a new one.

```rey
pub func clamp(value: float, min: float, max: float): float {
    if value < min {
        return min;
    } else if value > max {
        return max;
    }
    return value;
}
```

**3. Write a test** — see [Writing tests](#writing-tests)

**4. Update `src/math/README.md`** — add your function to the "what's implemented" list

**5. Open a PR** — see [Opening a PR](#opening-a-pr)

---

## Code style and conventions

- **camelCase** for functions and variables: `readFile`, `lineCount`
- **PascalCase** for structs: `FileHandle`, `HttpResponse`
- **SCREAMING_SNAKE_CASE** for constants: `MAX_BUFFER_SIZE`
- Always declare types explicitly on `pub func` — no implicit return types on public functions
- Keep functions small and single-purpose
- No magic numbers — use a named constant
- Error messages should be lowercase, specific, and actionable

```rey
// bad
pub func r(f: String): String { ... }

// good
pub func readFile(path: String): String { ... }
```

---

## Writing tests

Every function needs at least one test. Tests live in `tests/<module>/` and are named `<function>_test.rey`.

Example — `tests/math/clamp_test.rey`:

```rey
import std::math

var result = math.clamp(5.0, 0.0, 10.0);
assert(result == 5.0, "value within range should return unchanged");

result = math.clamp(-3.0, 0.0, 10.0);
assert(result == 0.0, "value below min should return min");

result = math.clamp(15.0, 0.0, 10.0);
assert(result == 10.0, "value above max should return max");

println("clamp: all tests passed");
```

Rules:
- Every `assert` needs a descriptive message — it shows up when the test fails
- Test edge cases: zero, negatives, empty strings, nulls where relevant
- End every test file with `println("<function>: all tests passed")`

Run your test:

```bash
rey tests/math/clamp_test.rey
```

---

## Opening a PR

Before opening a PR, confirm:
- [ ] Your function is marked `pub` in the right file
- [ ] You have at least one test in `tests/<module>/`
- [ ] Your test passes locally
- [ ] You updated the module README
- [ ] Your code follows the style guide above

**Branch naming:**
```
feat/math-clamp
fix/fs-read-null-path
docs/io-readme
```

**PR title format:**
```
feat(math): add clamp function
fix(fs): handle null path in readFile
docs(io): update README with missing functions
```

**PR description should answer:**
- What does this add or fix?
- Why is it needed?
- Are there any edge cases or known limitations?
- How was it tested?

If your PR is a work in progress, open it as a draft.

---

## Code of Conduct

Be kind. Be direct. Assume good intent.

- Critique code, never people
- If something is wrong, say so clearly and explain why
- If you're new and confused, ask — there are no stupid questions here
- If someone is being helpful, say thanks

We're building something together. Everyone who contributes in good faith is welcome here.

---

Questions? Open an issue or start a discussion on GitHub.