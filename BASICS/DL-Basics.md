# Deep Learning Basics

Deep learning is one of the main ideas behind many modern AI breakthroughs.

It is used in systems that can recognize images, translate languages, drive vehicles, play complex games, detect fake news, help diagnose diseases, and solve many other problems that were difficult to automate with traditional programming.

The central idea is that instead of manually writing every rule, we train a model to learn useful patterns directly from data.

# AI, Machine Learning, and Deep Learning

Artificial intelligence is the broad field of building systems that can perform tasks that seem intelligent.

Machine learning is a subset of artificial intelligence. It focuses on teaching computers to recognize patterns from data rather than explicitly programming every rule.

Deep learning is a subset of machine learning. It uses neural networks with many layers to learn representations directly from data.

The hierarchy is:

1. Artificial intelligence.
2. Machine learning.
3. Deep learning.

Deep learning is called "deep" because the data passes through multiple hidden layers. Each layer can learn a different level of representation.

# Why Deep Learning Matters

Traditional machine learning often requires a lot of human expertise.

For example, suppose we want a computer to recognize a face. A traditional approach might require humans to manually define features:

- A face has eyes.
- A face has ears.
- A face has a mouth.
- An eye has certain shapes and angles.
- A mouth has certain curves and positions.

This becomes difficult very quickly. Even defining something simple like an eye in exact computer rules is not easy.

Deep learning tries to avoid this manual feature engineering.

Instead of telling the computer exactly what features to look for, we give it many examples. A deep learning model can learn lower-level patterns such as edges and lines, then combine them into higher-level patterns such as eyes, mouths, and eventually full faces.

# Feature Learning

One of the most important strengths of deep learning is **feature learning**.

A deep learning model can learn useful features directly from raw data.

For an image model, the learning might look like this:

1. Early layers detect simple edges and lines.
2. Middle layers combine edges into shapes.
3. Later layers combine shapes into objects.
4. The final layer predicts the class, such as face, cat, dog, car, or truck.

This layered learning is why deep learning works well for complex data such as images, audio, video, and language.

# Why Deep Learning Became Popular

Many of the ideas behind deep learning existed for a long time.

Deep learning became much more successful later because three important things improved:

- **More data:** Modern systems have access to huge amounts of text, images, audio, video, and user-generated data.
- **Better hardware:** GPUs and specialized hardware made it possible to train large models much faster.
- **Better software:** Libraries such as TensorFlow and PyTorch made it easier to build, train, and deploy models.

Deep learning needs large amounts of data and computation. Once both became widely available, deep learning became practical.

# Neural Networks

Deep learning models are usually built from **neural networks**.

A neural network is inspired by the structure of the brain, but it is not a perfect copy of the brain. It is a mathematical system made of connected units called neurons.

A neural network learns by processing examples, comparing its predictions with the correct answers, and adjusting its internal parameters.

# Layers in a Neural Network

A basic neural network has three main kinds of layers:

- **Input layer:** Receives the original data.
- **Hidden layers:** Process the data and learn patterns.
- **Output layer:** Produces the final prediction.

For example, in a model that predicts whether a vehicle is a car or a truck:

- The input layer might receive vehicle weight and number of goods carried.
- The hidden layers learn relationships between those values.
- The output layer predicts whether the vehicle is more likely to be a car or truck.

# Neurons

A neuron receives numbers from the previous layer, combines them, and produces an output.

The basic computation is:

`z = w1x1 + w2x2 + ... + wnxn + b`

Then an activation function is applied:

`a = activation(z)`

Here:

- `x1, x2, ..., xn` are inputs.
- `w1, w2, ..., wn` are weights.
- `b` is the bias.
- `z` is the weighted sum plus bias.
- `a` is the activation or output of the neuron.

# Weights

Weights tell the network how important each input is.

If a weight is large, that input has a stronger influence on the neuron.

If a weight is small, that input has a weaker influence.

During training, the network changes its weights to improve its predictions.

# Bias

Bias is an extra number added to the weighted sum.

It lets the neuron shift its activation threshold.

The bias helps decide how easy or hard it is for a neuron to activate.

The neuron equation with bias is:

`z = w1x1 + w2x2 + ... + wnxn + b`

Without bias, the neuron is less flexible because its decision boundary is forced to depend only on the weighted inputs.

# Activation Functions

An activation function decides how a neuron transforms its weighted sum into an output.

Activation functions are important because they introduce **nonlinearity**.

Without nonlinear activation functions, a network with many layers would still behave like one large linear function. In that case, stacking many layers would not add much power.

Activation functions allow neural networks to learn complex patterns.

# Step Function

A step function activates a neuron only if the input is above a threshold.

For example:

`a = 1 if z > threshold`

`a = 0 otherwise`

This is simple, but it has problems.

If several output neurons all activate with value `1`, it becomes hard to tell which class should be chosen.

It is also difficult to train because the output jumps suddenly rather than changing smoothly.

# Linear Function

A linear activation gives output proportional to input.

For example:

`f(x) = mx + c`

The problem is that the derivative of a linear function is constant:

`f'(x) = m`

This means the gradient does not depend on the input value.

Another major problem is that stacking linear layers still produces a linear function. So even a deep network with many linear layers could be replaced by a single linear layer.

This is why deep networks need nonlinear activation functions.

# Sigmoid Function

The sigmoid function is a smooth nonlinear activation function.

It is commonly written as:

`sigmoid(x) = 1 / (1 + e^(-x))`

Sigmoid maps any input into a value between `0` and `1`.

This makes it useful when we want the output to behave like a probability.

Advantages of sigmoid:

- It is nonlinear.
- It gives smooth, gradual outputs.
- Its output is bounded between `0` and `1`.
- It can represent partial activation rather than only on or off.

Disadvantages of sigmoid:

- Very large positive or negative inputs produce very small gradients.
- Small gradients can make learning slow.
- This can lead to the **vanishing gradient problem**.

# Tanh Function

The tanh function is similar to sigmoid, but its output is between `-1` and `1`.

Like sigmoid:

- It is nonlinear.
- It is smooth.
- It is bounded.

The tanh function often has stronger gradients than sigmoid near the center, but it can still suffer from vanishing gradients at the extremes.

# ReLU Function

ReLU stands for **Rectified Linear Unit**.

It is defined as:

`ReLU(x) = max(0, x)`

This means:

- If `x` is negative, the output is `0`.
- If `x` is positive, the output is `x`.

ReLU is widely used because it is simple and efficient.

It also creates sparse activations because many neurons can output `0`. Sparse activations can make training more efficient.

One downside is that ReLU is not bounded above, so activations can become large.

# Forward Propagation

Forward propagation is the process of sending data from the input layer to the output layer.

The basic flow is:

1. Inputs enter the input layer.
2. Each input is multiplied by a weight.
3. The weighted values are added together.
4. A bias is added.
5. The result passes through an activation function.
6. The output moves to the next layer.
7. The process repeats until the output layer gives a prediction.

Forward propagation is how the network makes a prediction.

# Loss Function

A loss function measures how wrong the network's prediction is.

If the prediction is close to the correct answer, the loss is small.

If the prediction is far from the correct answer, the loss is large.

The loss function gives the network a numerical signal that says how badly it performed.

Training is the process of changing weights and biases so that the loss becomes smaller.

# Backpropagation

Backpropagation is the process that sends error information backward through the network.

After the network makes a prediction, the loss function measures the error. Backpropagation calculates how much each weight and bias contributed to that error.

This tells the model how to adjust its parameters.

The flow is:

1. Make a prediction with forward propagation.
2. Compare the prediction with the correct answer.
3. Calculate the loss.
4. Send error information backward through the layers.
5. Update weights and biases to reduce future loss.

Backpropagation is one of the main reasons neural networks can learn from data.

# Gradient Descent

Gradient descent is an optimization algorithm used to reduce the loss.

The idea is to adjust each parameter in the direction that makes the loss smaller.

For a weight:

`new_weight = old_weight - learning_rate * gradient`

For a bias:

`new_bias = old_bias - learning_rate * gradient`

The gradient tells which direction increases the loss. Subtracting the gradient moves the parameter in the opposite direction, toward lower loss.

# Learning Rate

The learning rate controls how large each update step is.

If the learning rate is too small, training may be very slow.

If the learning rate is too large, training may jump around and fail to settle into a good solution.

Choosing a learning rate is one of the important parts of tuning a model.

# Training Loop

A basic neural-network training loop looks like this:

1. Initialize weights and biases randomly.
2. Feed input data through the network.
3. Produce predictions.
4. Compare predictions with expected labels.
5. Calculate loss.
6. Use backpropagation to compute gradients.
7. Use gradient descent to update weights and biases.
8. Repeat until the model is good enough.

Each full pass through the training data is called an **epoch**.

# Vehicle Example

Suppose we have a dataset about vehicles.

Each example contains:

- Vehicle weight.
- Number of goods carried.
- Label: car or truck.

The network starts with random weights and biases.

For one example, such as weight `15` and goods `2`, the network performs forward propagation and makes a guess.

If it predicts truck but the correct answer is car, the loss function measures the error.

Backpropagation then adjusts the weights and biases so that the network becomes slightly better.

The same process repeats for many examples.

Over time, the network learns patterns that help it distinguish cars from trucks.

# Types of Learning

There are several major learning settings in machine learning and deep learning.

# Supervised Learning

In supervised learning, the training data includes correct answers.

For example:

- An image is labeled as cat or dog.
- A vehicle is labeled as car or truck.
- An email is labeled as spam or not spam.

The model learns by comparing its predictions with known labels.

# Unsupervised Learning

In unsupervised learning, the data does not include explicit labels.

The model tries to discover structure or patterns on its own.

Examples include:

- Grouping similar customers.
- Finding clusters in data.
- Discovering hidden patterns in large datasets.

# Reinforcement Learning

In reinforcement learning, an agent learns by taking actions and receiving rewards or penalties.

The goal is to learn a strategy that maximizes long-term reward.

This is useful in settings such as games, robotics, and decision-making systems.

# Neural Network Architectures

Different problems often need different neural-network architectures.

The architecture describes how the layers are arranged and how information flows through the network.

# Feedforward Neural Networks

A feedforward neural network sends information in one direction:

`input -> hidden layers -> output`

There are no loops.

Feedforward networks are useful for many basic prediction and classification tasks.

# Convolutional Neural Networks

Convolutional neural networks, or CNNs, are especially useful for image-related tasks.

CNNs are built to process grid-like data such as images.

An image can be represented as a matrix of pixel values. A color image usually has three channels:

- Red.
- Green.
- Blue.

CNNs use special layers to extract visual features from images.

# Convolution

Convolution is a technique for detecting local patterns in an image.

A small filter, also called a kernel, moves across the image.

At each position, the filter looks at a small region and performs a mathematical operation to produce a feature value.

Different filters can learn to detect different features, such as:

- Edges.
- Corners.
- Textures.
- Shapes.

Early convolution layers may detect simple visual patterns. Later layers can combine them into more complex objects.

# Pooling

Pooling reduces the size of feature maps while keeping the most important information.

This is also called subsampling or downsampling.

Common pooling methods include:

- **Max pooling:** Keeps the largest value in a selected region.
- **Min pooling:** Keeps the smallest value in a selected region.

Pooling helps reduce computation and can make the model more robust to small shifts in the image.

# Typical CNN Flow

A common CNN for image classification works like this:

1. Input image enters the network.
2. Convolution layers apply filters and produce feature maps.
3. Pooling layers reduce the size of the feature maps.
4. Convolution and pooling may repeat several times.
5. Fully connected layers combine the learned features.
6. The output layer predicts the image class.

CNNs are used in:

- Image recognition.
- Image processing.
- Image segmentation.
- Video analysis.
- Some natural language processing tasks.

# Steps in a Deep Learning Project

Most deep learning projects follow a common workflow.

The five broad steps are:

1. Gather data.
2. Prepare and preprocess data.
3. Train the model.
4. Evaluate the model.
5. Improve and optimize the model.

# Gathering Data

Data is central to deep learning.

A model can only learn from the data it receives.

Bad data usually leads to a bad model.

When gathering data, consider:

- What problem is being solved.
- What examples are needed.
- How much data is needed.
- Whether the labels are reliable.
- Whether the data contains noise.
- Whether the data represents the real-world situation.

There is no single perfect amount of data for every problem.

In general, larger and more complex models need more data.

# Data Quality

Quantity matters, but quality matters too.

A reliable dataset is more likely to produce a useful model.

Common data quality questions include:

- Are the labels correct?
- Are there many missing values?
- Are the features noisy?
- Does the dataset represent the real problem?
- Are some classes overrepresented?

Deep learning can handle some noise, but poor data quality can seriously limit model performance.

# Train, Validation, and Test Sets

Datasets are often split into three parts:

- **Training set:** Used to train the model.
- **Validation set:** Used to tune the model and choose hyperparameters.
- **Test set:** Used at the end to estimate real-world performance.

The validation set matters because model development involves repeated choices, such as changing architecture, learning rate, or number of epochs.

The test set should be saved for the final evaluation.

# Cross Validation

Cross validation creates multiple training and validation splits from the available data.

The model is trained and validated on different splits.

This gives a more reliable estimate of performance and helps reduce overfitting to one particular validation split.

One common method is **k-fold cross validation**.

# Time-Based Splits

For time-series data, it often makes sense to split by time.

For example, with 40 days of data:

- Train on days 1 to 39.
- Evaluate on day 40.

This better matches real-world prediction, where the model is trained on older data and used on future data.

# Formatting Data

Data may not arrive in the format needed by the model.

It might come as:

- CSV files.
- Databases.
- Images.
- JSON files.
- Text files.

Formatting means converting the data into a form the training pipeline can use.

# Missing Data

Real-world datasets often contain missing values.

Missing values may appear because of:

- Collection errors.
- Blank survey fields.
- Measurements that were not applicable.
- System failures.

Missing values are often represented as `NaN` or `null`.

Most algorithms cannot directly handle missing values, so they must be addressed before training.

Common approaches include:

- Removing rows or features with missing values.
- Filling missing values with a mean, median, mode, or other estimate.

Removing data can discard useful information. Filling missing values incorrectly can introduce misleading patterns.

# Sampling Large Data

Sometimes there is more data than is practical to process during early development.

In that case, it can be useful to work with a smaller sample first.

This helps with:

- Faster experimentation.
- Lower memory use.
- Quicker debugging.
- Easier prototyping.

Once the pipeline works, the model can be trained on more data.

# Imbalanced Data

Classification datasets often have imbalanced classes.

For example, one class may appear much more often than another.

If a model trains on heavily imbalanced data, it may learn to favor the majority class and ignore the minority class.

Ways to address imbalance include:

- Downsampling the majority class.
- Upsampling the minority class.
- Adding example weights.

Downsampling reduces the number of majority-class examples.

Upweighting gives more importance to selected examples during training.

These methods help the model pay more attention to minority classes.

# Feature Scaling

Feature scaling puts input features on similar numerical scales.

Many deep learning algorithms train better when features are scaled.

Two common methods are normalization and standardization.

# Normalization

Normalization rescales values to a fixed range, often `0` to `1`.

One common method is min-max scaling:

`x_scaled = (x - x_min) / (x_max - x_min)`

This keeps all values within a shared range.

# Standardization

Standardization shifts values so that they have mean `0` and standard deviation `1`.

The formula is:

`x_standardized = (x - mean) / standard_deviation`

Standardization can make training easier because features have similar distributions.

It also keeps information about outliers better than simple min-max normalization in some cases.

# Model Evaluation

After training, the model must be evaluated on data it has not seen during training.

Evaluation estimates how well the model might perform in the real world.

A model that performs well only on training data is not enough. It must generalize to new examples.

# Hyperparameters

Hyperparameters are settings chosen before or during training that are not learned in the same way as weights and biases.

Examples include:

- Learning rate.
- Number of epochs.
- Number of layers.
- Number of neurons.
- Batch size.
- Regularization strength.

Tuning hyperparameters can improve model performance, but it is often experimental.

# Overfitting

Overfitting happens when a model performs well on training data but poorly on unseen data.

This means the model has learned details or noise that are specific to the training set rather than general patterns.

For example, an overfit image model might memorize specific training images instead of learning the general idea of what a cat or dog looks like.

# Underfitting

Underfitting happens when a model is too simple to learn the important patterns in the data.

An underfit model performs poorly on both training data and unseen data.

There is a balance:

- Too much capacity can lead to overfitting.
- Too little capacity can lead to underfitting.

# Reducing Overfitting

Common ways to reduce overfitting include:

- Getting more data.
- Reducing model size.
- Using regularization.
- Using data augmentation.
- Using dropout.

# Regularization

Regularization adds a penalty for overly complex models.

One common approach is to penalize large weights.

This encourages the model to use smaller, simpler weights and can improve generalization.

# L1 and L2 Regularization

L1 regularization adds a cost based on the absolute value of weights.

`L1 penalty = lambda * sum(abs(weights))`

L2 regularization adds a cost based on the squared value of weights.

`L2 penalty = lambda * sum(weights^2)`

Here:

- `lambda` controls how strong the regularization is.
- Larger `lambda` means a stronger penalty.

# Data Augmentation

Data augmentation creates modified versions of existing training examples.

For images, this might include:

- Flipping.
- Rotating.
- Zooming.
- Cropping.
- Blurring.

The goal is to expose the model to more variation without collecting entirely new data.

This can help the model generalize better.

# Dropout

Dropout randomly ignores some neurons during training.

During a particular forward and backward pass, selected neurons are temporarily removed from the computation.

This prevents neurons from becoming too dependent on each other.

Dropout is especially useful in large fully connected layers, where many parameters can lead to overfitting.

# Key Idea

Deep learning trains neural networks to learn useful patterns directly from data.

The basic cycle is:

1. Send inputs forward through the network.
2. Produce a prediction.
3. Measure the error with a loss function.
4. Send error information backward with backpropagation.
5. Update weights and biases with gradient descent.
6. Repeat until the model performs well.

Deep learning works well because layered neural networks can learn increasingly complex representations, especially when given enough data, computation, and careful training.
