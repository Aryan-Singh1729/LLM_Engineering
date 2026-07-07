# Handwritten Digit Recognition

A sloppily written `3`, rendered at an extremely low resolution of 28 × 28 pixels, is still easy for the brain to recognize as a `3`.

Many differently written images are also recognizable as `3`s even though the specific value of each pixel is very different from one image to the next. The particular light-sensitive cells in the eye that fire for one `3` are very different from those that fire for another. Something in the visual cortex resolves these images as representing the same idea while recognizing other images as distinct ideas.

## Programming Situation

Writing a program that takes a grid of 28 × 28 pixels and outputs a single number telling you what it thinks the digit is turns a comically trivial task for the brain into a dauntingly difficult one.

The neural network in the video is built to learn to recognize handwritten digits. This is a classic example for introducing neural networks.

# Neural Networks

Neural networks are inspired by the brain. The important questions are what the neurons are and in what sense they are linked together.

There are many variants of neural networks, with a boom in research toward these variants in recent years. The video examines the simplest plain vanilla form with no added frills. This form is a necessary prerequisite for understanding more powerful modern variants, and even this simplest form can learn to recognize handwritten digits.

The video focuses on the structure of a neural network. Learning is handled in the following video.

# Neurons

A neuron is a thing that holds a number, specifically a number between 0 and 1.

It is more accurate to think of each neuron as a function. It takes the outputs of all the neurons in the previous layer and produces a number between 0 and 1. The specific number it holds depends on the image fed into the network.

# Activations

The number inside a neuron is called its **activation**. A neuron can be pictured as lit up when its activation is a high number.

The activations in one layer determine the activations of the next layer. The heart of the network as an information-processing mechanism is how the activations from one layer bring about activations in the next layer.

This is loosely analogous to how some groups of neurons firing in biological networks cause certain other neurons to fire.

# Input Layer

The input layer contains one neuron for each pixel in the 28 × 28 input image:

`28 × 28 = 784 neurons`

Each neuron holds a number representing the grayscale value of its corresponding pixel:

- `0` represents a black pixel.
- `1` represents a white pixel.

# Output Layer

The final layer contains 10 neurons, with each neuron representing one digit.

The activation of each output neuron is a number between 0 and 1 representing how much the system thinks the image corresponds to that digit.

The brightest neuron in the output layer is the network's choice for the digit represented by the image.

# Hidden Layers

The layers between the input and output are called **hidden layers**.

The network in the video has two hidden layers with 16 neurons each. Two layers were chosen to motivate the structure, and 16 was a convenient number to fit on the screen. In practice, there is a lot of room to experiment with the specific structure.

# How Information Moves Through the Network

When an image is fed into the trained network:

1. All 784 neurons in the input layer light up according to the brightness of their corresponding pixels.
2. This pattern of activations causes a specific pattern in the next layer.
3. That pattern causes another pattern in the following layer.
4. A final pattern appears in the output layer.
5. The brightest output neuron represents the network's choice of digit.

# Layers of Abstraction

A layered structure might behave intelligently by breaking a problem into components and subcomponents.

## Digit Component Examples

- A `9` has a loop at the top and a line on the right.
- An `8` also has a loop at the top, paired with another loop lower down.
- A `4` can be broken down into three specific lines.

In a perfect world, each neuron in the second-to-last layer might correspond to one of these subcomponents. Whenever an image has a loop at the top, such as a `9` or an `8`, a specific neuron might have an activation close to 1.

The hope is not that the neuron recognizes only one specific loop of pixels. Any generally loopy pattern near the top should activate it. Moving from the third layer to the output layer would then require learning which combinations of subcomponents correspond to which digits.

# Recognizing Edges

Recognizing a loop can be broken into the smaller problem of recognizing the little edges that make it up.

A long line, like one found in the digits `1`, `4`, or `7`, is a long edge or a particular pattern of several smaller edges.

The hope might be that each neuron in the second layer corresponds to a relevant little edge.

## Recognizing a 9 Situation

An image of a `9` might:

1. Activate neurons associated with around 8 to 10 specific little edges.
2. These neurons might activate neurons associated with an upper loop and a long vertical line.
3. Those neurons might activate the output neuron associated with `9`.

Whether the final trained network actually behaves this way is a separate question, but it is a possible goal for the layered structure.

Detecting edges and patterns would also be useful for other image-recognition tasks.

# Speech Recognition Example

Intelligent tasks beyond image recognition can also break down into layers of abstraction.

Parsing speech involves:

1. Taking raw audio and identifying distinct sounds.
2. Combining sounds to make syllables.
3. Combining syllables to form words.
4. Combining words to make phrases and more abstract thoughts.

# Weights

A weight is assigned to every connection between a neuron and the neurons in the previous layer. Weights are numbers.

The activations from the previous layer are combined by computing their weighted sum according to these weights.

Weights can be pictured in a grid:

- Green pixels represent positive weights.
- Red pixels represent negative weights.
- The brightness of a pixel loosely represents the value of the weight.

The weights tell you what pixel pattern a neuron in the second layer is picking up on.

## Detecting an Edge in a Specific Region

Suppose one neuron in the second layer should detect whether an image has an edge in a particular region.

If the weights for almost every pixel are zero, except for positive weights in the region of interest, the weighted sum amounts to adding the pixel values only in that region.

Negative weights can be assigned to the surrounding pixels. The sum is then largest when the middle pixels are bright and the surrounding pixels are darker.

# Weighted Sum

A weighted sum combines activations from the previous layer with their corresponding weights.

For one neuron in the next layer, let the activations in the previous layer be:

$$
a_1,a_2,\ldots,a_n
$$

and let the weights connecting those neurons to the new neuron be:

$$
w_1,w_2,\ldots,w_n
$$

The weighted sum is:

$$
w_1a_1+w_2a_2+\cdots+w_na_n
$$

Using summation notation, this is:

$$
\sum_{j=1}^{n}w_ja_j
$$

For a particular neuron numbered $i$ in the next layer, every connection has its own weight $w_{ij}$. Its weighted sum is:

$$
z_i=\sum_{j=1}^{n}w_{ij}a_j
$$

Here, $j$ identifies a neuron in the previous layer and $i$ identifies the neuron whose activation is being calculated.

A weighted sum can produce any number, but the network in the video requires activations between 0 and 1.

# Sigmoid Function

A common way to turn a weighted sum into an activation between 0 and 1 is to pass it through the **sigmoid function**, also known as a **logistic curve**.

The sigmoid function squishes the real number line into the range between 0 and 1:

- Very negative inputs produce values close to 0.
- Positive inputs produce values close to 1.
- The function steadily increases around the input 0.

The sigmoid function is written as:

$$
\sigma(z)=\frac{1}{1+e^{-z}}
$$

Without a bias, the activation of neuron $i$ would be:

$$
a_i'=\sigma\left(\sum_{j=1}^{n}w_{ij}a_j\right)
$$

The activation of a neuron is therefore a measure of how positive the relevant weighted sum is.

# Biases

A neuron may not need to activate merely when its weighted sum is greater than 0. It might need to activate only when the sum is greater than a value such as 10.

To create this bias toward inactivity, another number, such as `-10`, is added to the weighted sum before it is passed through the sigmoid function. This additional number is called the **bias**.

The weights determine the pixel pattern a neuron detects. The bias determines how high the weighted sum must be before the neuron becomes meaningfully active.

After adding a bias $b_i$, the value before sigmoid is:

$$
z_i=w_{i1}a_1+w_{i2}a_2+\cdots+w_{in}a_n+b_i
$$

or equivalently:

$$
z_i=\sum_{j=1}^{n}w_{ij}a_j+b_i
$$

The new activation is:

$$
a_i'=\sigma(z_i)
$$

Combining these steps gives the complete equation for one neuron:

$$
a_i'=\sigma\left(\sum_{j=1}^{n}w_{ij}a_j+b_i\right)
$$

For the example bias of $-10$ mentioned in the video:

$$
a_i'=\sigma\left(\sum_{j=1}^{n}w_{ij}a_j-10\right)
$$

This makes the weighted sum need to become roughly greater than 10 before the input to sigmoid becomes positive.

# Number of Weights and Biases

Every neuron in the first hidden layer is connected to all 784 pixel neurons in the input layer. Every connection has its own weight, and every hidden neuron has a bias.

For a hidden layer with 16 neurons, the connection between the input and first hidden layer contains:

- `784 × 16` weights.
- `16` biases.

The connections between the other layers also contain weights and biases.

Altogether, the network has almost exactly 13,000 weights and biases. These are 13,000 knobs and dials that can be adjusted to make the network behave in different ways.

## Exact Parameter Count

From the 784-neuron input layer to the first 16-neuron hidden layer:

$$
784\times16=12{,}544\text{ weights}
$$

$$
16\text{ biases}
$$

From the first 16-neuron hidden layer to the second 16-neuron hidden layer:

$$
16\times16=256\text{ weights}
$$

$$
16\text{ biases}
$$

From the second 16-neuron hidden layer to the 10-neuron output layer:

$$
16\times10=160\text{ weights}
$$

$$
10\text{ biases}
$$

The total number of weights is:

$$
12{,}544+256+160=12{,}960
$$

The total number of biases is:

$$
16+16+10=42
$$

Therefore, the complete number of parameters is:

$$
12{,}960+42=13{,}002
$$

This is the almost exactly 13,000 weights and biases described in the video.

# Learning

Learning refers to getting the computer to find a valid setting for all the weights and biases so that the network solves the problem at hand.

## Setting Parameters by Hand Thought Experiment

Imagine sitting down and setting all the weights and biases by hand. You would deliberately adjust the numbers so that the second layer detects edges, the third layer detects patterns, and so on.

Understanding what weights and biases mean provides a starting place for experimenting with the structure when a network does not perform as anticipated.

When a network works for reasons other than those expected, examining its weights and biases can challenge assumptions and expose the full space of possible solutions.

# Vector of Activations

The activations from one layer can be organized into a column called a **vector**.

For a layer containing $n$ neurons, its activation vector is:

$$
\mathbf{a}=
\begin{bmatrix}
a_1\\
a_2\\
\vdots\\
a_n
\end{bmatrix}
$$

The 784 input-pixel activations form a $784\times1$ column vector:

$$
\mathbf{a}^{(0)}=
\begin{bmatrix}
a_1^{(0)}\\
a_2^{(0)}\\
\vdots\\
a_{784}^{(0)}
\end{bmatrix}
$$

Each number in this vector is the grayscale value of one pixel, from 0 for black to 1 for white.

# Weight Matrix

All the weights can be organized into a **matrix**. Each row of the matrix corresponds to the connections between one layer and a particular neuron in the next layer.

If the previous layer has $n$ neurons and the next layer has $m$ neurons, the weight matrix is:

$$
\mathbf{W}=
\begin{bmatrix}
w_{11} & w_{12} & \cdots & w_{1n}\\
w_{21} & w_{22} & \cdots & w_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
w_{m1} & w_{m2} & \cdots & w_{mn}
\end{bmatrix}
$$

This matrix has $m$ rows and $n$ columns. Row $i$ contains all the weights leading into neuron $i$ of the next layer.

Taking the weighted sum of the activations in the first layer corresponds to a term in the matrix-vector product.

The matrix-vector multiplication is:

$$
\mathbf{W}\mathbf{a}=
\begin{bmatrix}
w_{11} & w_{12} & \cdots & w_{1n}\\
w_{21} & w_{22} & \cdots & w_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
w_{m1} & w_{m2} & \cdots & w_{mn}
\end{bmatrix}
\begin{bmatrix}
a_1\\
a_2\\
\vdots\\
a_n
\end{bmatrix}
=
\begin{bmatrix}
w_{11}a_1+w_{12}a_2+\cdots+w_{1n}a_n\\
w_{21}a_1+w_{22}a_2+\cdots+w_{2n}a_n\\
\vdots\\
w_{m1}a_1+w_{m2}a_2+\cdots+w_{mn}a_n
\end{bmatrix}
$$

Each entry of the resulting vector is the weighted sum for one neuron in the next layer.

Much of machine learning comes down to having a good grasp of linear algebra.

# Bias Vector

Instead of adding every bias to its corresponding value independently, all the biases can be organized into a vector. The entire bias vector is added to the matrix-vector product.

For a layer with $m$ neurons, the bias vector is:

$$
\mathbf{b}=
\begin{bmatrix}
b_1\\
b_2\\
\vdots\\
b_m
\end{bmatrix}
$$

Adding it to the matrix-vector product gives:

$$
\mathbf{z}=\mathbf{W}\mathbf{a}+\mathbf{b}
$$

The sigmoid function is then applied to every component of the resulting vector.

Therefore, the full transition from one layer to the next is:

$$
\mathbf{a}'=\sigma(\mathbf{W}\mathbf{a}+\mathbf{b})
$$

The sigmoid is applied separately to every entry:

$$
\mathbf{a}'=
\begin{bmatrix}
\sigma(z_1)\\
\sigma(z_2)\\
\vdots\\
\sigma(z_m)
\end{bmatrix}
$$

## Matrix Dimensions in the Digit Network

The transition from the 784-neuron input layer to the first 16-neuron hidden layer uses:

$$
\mathbf{W}^{(1)}_{16\times784}\mathbf{a}^{(0)}_{784\times1}+\mathbf{b}^{(1)}_{16\times1}
$$

and produces:

$$
\mathbf{a}^{(1)}_{16\times1}=\sigma\left(\mathbf{W}^{(1)}\mathbf{a}^{(0)}+\mathbf{b}^{(1)}\right)
$$

The transition from the first 16-neuron hidden layer to the second 16-neuron hidden layer is:

$$
\mathbf{a}^{(2)}_{16\times1}=\sigma\left(\mathbf{W}^{(2)}_{16\times16}\mathbf{a}^{(1)}_{16\times1}+\mathbf{b}^{(2)}_{16\times1}\right)
$$

The transition from the second 16-neuron hidden layer to the 10-neuron output layer is:

$$
\mathbf{a}^{(3)}_{10\times1}=\sigma\left(\mathbf{W}^{(3)}_{10\times16}\mathbf{a}^{(2)}_{16\times1}+\mathbf{b}^{(3)}_{10\times1}\right)
$$

The complete network repeatedly applies the same matrix, bias, and sigmoid operation:

$$
\mathbf{a}^{(0)}
\longrightarrow
\mathbf{a}^{(1)}
\longrightarrow
\mathbf{a}^{(2)}
\longrightarrow
\mathbf{a}^{(3)}
$$

Writing the weight matrix, activation vector, and bias vector as symbols expresses the full transition from one layer to the next in a tight form.

This notation also makes the relevant code simpler and faster because many libraries heavily optimize matrix multiplication.

# A Neural Network as a Function

The entire network is a function that takes 784 numbers as input and produces 10 numbers as output.

It is an extremely complicated function involving:

- Almost 13,000 parameters in the form of weights and biases.
- Parameters that pick up on certain patterns.
- Repeated matrix-vector products.
- The sigmoid function.

Despite this complexity, it is still a function. Its complexity gives it hope of taking on the challenge of recognizing handwritten digits.

# Limitations and Open Questions

Even the simplest network can recognize handwritten digits, but it falls short of some hopes one might have for it.

The video leaves the following questions for the discussion of training:

- How does the network learn appropriate weights and biases by looking at data?
- Does the trained network actually detect edges, patterns, and digit components in the hoped-for way?

# Sigmoid in Early Neural Networks

Early networks used sigmoid to squish a weighted sum into the interval between 0 and 1. This was motivated by the biological analogy of neurons being inactive or active.

Relatively few modern networks use sigmoid in this way. Sigmoid made training difficult at some point, while ReLU proved to work well for incredibly deep neural networks.

# ReLU

**ReLU** stands for **rectified linear unit**.

For an input `a`, ReLU takes the maximum of `0` and `a`:

`ReLU(a) = max(0, a)`

If the input passes the threshold, ReLU acts as the identity function. If it does not, the neuron is not activated and its output is 0.

This was motivated partly by the biological analogy that neurons either activate or do not activate. ReLU is also a simplification and seems to be much easier to train than sigmoid.

# Resources Mentioned

The video mentions:

- A following video about how a neural network learns and what this particular network is really doing.
- Resources where viewers can learn more, download the code, and experiment with it on their own computers.
- A visual linear algebra series, especially Chapter 3, for understanding matrices and matrix-vector multiplication.
- A probability series that the creator planned to return to after this project.

# YouTube Recommendation Algorithm Situation

Subscribing may prime the neural networks underlying YouTube's recommendation algorithm to believe that a viewer wants content from the channel recommended to them.

# Closing Discussion

Lisha Li did her PhD work on the theoretical side of deep learning and worked at the venture capital firm Amplify Partners, which provided some funding for the video.

In the closing discussion, she explains that relatively few modern networks use sigmoid, that ReLU seems easier to train, and that people tried ReLU after sigmoid became difficult to use for training. ReLU happened to work very well for incredibly deep neural networks.
