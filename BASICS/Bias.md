# Bias

Bias is a small but very important idea in neural networks.

At a high level, a bias is an extra number that lets a neuron shift its decision boundary. It gives the model flexibility beyond simply multiplying inputs by weights.

Weights decide how strongly each input matters. Bias decides how easy or hard it is for a neuron to activate.

# Simple Everyday Idea

Imagine a light that turns on when enough people enter a room.

If the rule is:

`turn on when the number of people is greater than 5`

then the light stays off for 1, 2, 3, 4, or 5 people, and turns on for 6 or more.

The number `5` acts like a threshold.

In a neural network, a bias helps control this kind of threshold. It shifts when a neuron becomes active.

# Simple Number Example

Suppose a neuron receives one input:

`x = number of people in a room`

The neuron computes:

`output = x`

If this output is passed through a rule that activates when the value is greater than `0`, then the neuron activates for almost every positive input.

That is not very flexible.

Now suppose the neuron computes:

`output = x - 5`

Now the neuron activates only when:

`x - 5 > 0`

which means:

`x > 5`

The `-5` is acting like a bias. It shifts the activation point from `0` to `5`.

# Bias as an Extra Adjustment

A simple neuron usually computes a weighted sum of its inputs.

For one input, this looks like:

`z = wx`

Here:

- `x` is the input.
- `w` is the weight.
- `z` is the value before activation.

With a bias, the equation becomes:

`z = wx + b`

Here:

- `b` is the bias.

The bias is added after multiplying the input by the weight.

# What the Bias Does

The bias shifts the value of `z`.

If the bias is positive, it makes the neuron more likely to activate.

If the bias is negative, it makes the neuron less likely to activate.

For example:

`z = 2x`

and

`z = 2x + 10`

have the same weight, but the second equation is shifted upward by `10`.

Similarly:

`z = 2x - 10`

is shifted downward by `10`.

# Line Example

The equation:

`y = wx`

always passes through the origin, meaning the point `(0, 0)`.

For example:

`y = 2x`

passes through `(0, 0)`.

But many useful patterns do not pass through the origin.

The equation:

`y = wx + b`

can shift the line up or down.

For example:

`y = 2x + 3`

has the same slope as `y = 2x`, but it crosses the y-axis at `3`.

That extra `+3` is the bias.

So in simple geometry:

- The weight controls the slope.
- The bias controls the intercept.

# Bias in a Neuron

A neuron takes inputs from the previous layer and produces an activation.

Before the activation function is applied, the neuron computes:

`z = w1x1 + w2x2 + w3x3 + ... + b`

Here:

- `x1, x2, x3, ...` are inputs.
- `w1, w2, w3, ...` are weights.
- `b` is the bias.
- `z` is the weighted sum plus bias.

Then the neuron applies an activation function:

`a = sigma(z)`

Here:

- `a` is the neuron's activation.
- `sigma` represents an activation function, such as sigmoid, ReLU, or another nonlinearity.

So the full neuron can be written as:

`a = sigma(w1x1 + w2x2 + w3x3 + ... + b)`

# Sigmoid Function

The **sigmoid function** is one example of an activation function.

An activation function takes the neuron's raw value, usually called `z`, and turns it into the neuron's final activation `a`.

The sigmoid function is written as:

`sigmoid(z) = 1 / (1 + e^(-z))`

It converts any input number into a value between `0` and `1`.

For example:

- If `z` is a large positive number, `sigmoid(z)` is close to `1`.
- If `z` is `0`, `sigmoid(z)` is exactly `0.5`.
- If `z` is a large negative number, `sigmoid(z)` is close to `0`.

So sigmoid behaves like a smooth version of an on/off switch.

Instead of jumping suddenly from `0` to `1`, it gradually changes:

- Very negative input gives a low activation.
- Input near `0` gives a medium activation.
- Very positive input gives a high activation.

# Why Sigmoid Is Used

Sigmoid is useful when we want a neuron's output to behave like a probability or confidence score.

Because its output is always between `0` and `1`, it can be interpreted as something like:

- `0` means very inactive or very unlikely.
- `0.5` means uncertain or halfway active.
- `1` means very active or very likely.

Sigmoid was especially common in earlier neural networks because it gives a simple, smooth way to turn a weighted sum into an activation.

The smoothness matters during training. Since sigmoid changes gradually, the network can calculate how small changes in weights and biases affect the final output. This is important for gradient descent.

Modern neural networks often use other activation functions, such as ReLU, in hidden layers. But sigmoid is still very useful for explaining how bias shifts activation thresholds because its midpoint is easy to see:

`sigmoid(0) = 0.5`

That means the neuron is halfway active when its pre-activation value `z` is `0`.

# Bias and Activation Thresholds

Suppose a neuron uses a sigmoid activation function.

Since the sigmoid function is centered around `0`, we know:

`sigmoid(0) = 0.5`

If:

`z = wx`

then the neuron reaches the midpoint activation when:

`wx = 0`

But if:

`z = wx + b`

then the midpoint happens when:

`wx + b = 0`

Solving for `x`:

`wx = -b`

`x = -b / w`

This shows that the bias changes the input value where the neuron reaches its midpoint.

In other words, the bias shifts the activation threshold.

# Example With a Sigmoid

Suppose:

`z = 2x - 6`

The neuron reaches its midpoint when:

`2x - 6 = 0`

`2x = 6`

`x = 3`

So this neuron starts becoming strongly active around `x = 3`.

The weight `2` controls how sharply the value changes as `x` changes.

The bias `-6` shifts the point where the neuron becomes active.

# Bias With Multiple Inputs

For multiple inputs, the equation is:

`z = w1x1 + w2x2 + ... + wnxn + b`

This can also be written using vectors:

`z = dot(w, x) + b`

Here:

- `w` is the weight vector.
- `x` is the input vector.
- `dot(w, x)` is the dot product.
- `b` is the bias.

The dot product measures how much the input points in the direction the neuron cares about.

The bias then shifts the result before the activation function is applied.

# Geometric Meaning

In two dimensions, a neuron can separate points with a line.

Without a bias, the boundary has to pass through the origin.

With a bias, the boundary can move away from the origin.

For a simple decision boundary:

`w1x1 + w2x2 + b = 0`

the weights control the orientation of the boundary, while the bias controls its position.

This makes the neuron much more flexible.

# Bias in a Layer

A layer usually contains many neurons.

Each neuron has its own bias.

For a whole layer, the equation is:

`z = Wx + b`

Here:

- `x` is the input vector.
- `W` is the weight matrix.
- `b` is the bias vector.
- `z` is the vector of pre-activation values.

Then the activation function is applied:

`a = sigma(z)`

or:

`a = sigma(Wx + b)`

The bias vector has one bias value for each neuron in the layer.

# Why Bias Is Needed

Bias gives the network more freedom.

Without bias, every neuron would be forced to behave as if `0` were a special fixed reference point.

This can make learning harder or even prevent the network from representing simple patterns efficiently.

Bias allows the network to:

- Shift activation thresholds.
- Move decision boundaries.
- Represent patterns that do not pass through the origin.
- Learn useful default tendencies even when inputs are zero.

# Bias During Training

Bias values are learned during training, just like weights.

At the beginning, biases may be initialized to `0` or small values.

During training, the model adjusts both weights and biases to reduce the loss.

For a parameter update, the idea is:

`new bias = old bias - learning rate * gradient`

More formally:

`b := b - learning_rate * dL/db`

Here:

- `b` is the bias.
- `learning_rate` controls the size of the update step.
- `L` is the loss function.
- `dL/db` tells how much the loss changes when the bias changes.

If changing the bias upward reduces the loss, training will push it upward.

If changing the bias downward reduces the loss, training will push it downward.

# Bias Is Not the Same as Statistical Bias

In machine learning, the word **bias** can mean different things.

In neural-network equations, a bias is a learned parameter added to a weighted sum.

This is different from social, statistical, or dataset bias.

For example:

- A neural-network bias is the `b` in `z = wx + b`.
- Dataset bias means the training data does not fairly represent the real world.
- Social bias means a model may reflect unfair or harmful patterns from its data.

These ideas are related only by name. In this note, bias means the mathematical parameter inside a neuron.

# Key Idea

Bias is an added learnable number that shifts a neuron's output before the activation function.

Weights control how inputs influence the neuron. Bias controls where the neuron activates.

The basic equation is:

`a = sigma(dot(w, x) + b)`

This small `+ b` gives neural networks much more flexibility, allowing them to move thresholds and decision boundaries instead of forcing everything to pass through the origin.
