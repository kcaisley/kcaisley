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


## Computing
Older methods, like analog computers/slides rules are approximate because of error accumulation from mechanical tolerances and electronic noise. And so the methods explored by them accumulate error in a different manner.

Digital computers instead accumulate error through quantization. Herein lies the beauty of digital; after conversion, it suppress analog noise/error with noise margin, at the expense of quantization error. Note: Computers drastically accelerate numerical **AND** symbolic methods. Therefore, make sure you don’t confuse numerical(approximate) methods are being the exclusive or sole domain of digital computers. Humans can also do numerical and symbolic computation. (Is analog computation a third type?)

Recall: Algorithms, numerical/approximate methods, which existed before Computer science and programming.