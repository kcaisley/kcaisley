I'm calling this computing, as:

- 'Software' is too general, as it would include use of GUI applications, etc
- 'Programming' is too general, as this would including embedded systems, graphics, etc.
- My focus is on scientific and engineering computing and tools

```julia
using Plots
x = range(0, 10, length=100)
y = sin.(x)
plot(x, y)
```