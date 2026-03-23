# std::math

Math utilities for Rey.

## Implemented

### basic.rey
- `clamp(value, min, max)`
- `sign(n)`
- `round(n)`
- `floor(n)`
- `ceil(n)`

### trig.rey
- `PI`
- `sin(x)` (Taylor series approximation)
- `cos(x)` (Taylor series approximation)
- `tan(x)` (computed as `sin(x) / cos(x)`)
- `toRadians(deg)`
- `toDegrees(rad)`

### stats.rey
- `sum(nums: ...int)`
- `mean(nums: ...int)`
- `max(a, b)`
- `min(a, b)`
- `abs(n)`
