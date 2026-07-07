# GPT

GPT stands for **Generative Pretrained Transformer**.

- **Generative:** These bots generate new text.
- **Pretrained:** The model has already learned from a massive amount of data. The prefix suggests that it can later be fine-tuned for specific tasks with additional training.
- **Transformer:** A transformer is a specific kind of neural network and machine-learning model. It is the core invention underlying the current boom in AI.

# Transformer Applications

Many kinds of models can be built using transformers:

- Some models take audio as input and produce a transcript.
- Other models go in the opposite direction and produce synthetic speech from text.
- Tools such as DALL-E and Midjourney take a text description and produce an image.
- The original transformer introduced by Google in 2017 was created to translate text from one language into another.
- The transformer variant underlying tools such as ChatGPT takes in text, possibly accompanied by images or sound, and predicts what comes next in the passage.

# Next-Token Prediction

A transformer used for text generation produces a probability distribution over many possible chunks of text that might follow the input.

Predicting the next token may initially seem different from generating text, but a prediction model can generate a longer passage through repeated prediction and sampling:

1. Give the model an initial snippet of text.
2. Let it produce a probability distribution for what comes next.
3. Randomly sample a token from that distribution.
4. Append the sampled token to the text.
5. Run the process again using all the text, including the newly added token.
6. Repeat.

This repeated prediction and sampling is essentially what happens when ChatGPT and other large language models appear to produce one word at a time.

## GPT-2 and GPT-3 Story Example

When GPT-2 was run on a laptop and repeatedly asked to predict and sample the next chunk of a story from seed text, the resulting story did not make much sense.

When GPT-2 was replaced by API calls to GPT-3—the same basic model, but much larger—the result became a sensible story. It even appeared to infer that a pi creature would live in a land of mathematics and computation.

One useful feature would be the ability to see the underlying probability distribution for every new word the model chooses.

# High-Level Data Flow Through a Transformer

When a chatbot generates a word, the following process happens:

1. The input is broken into tokens.
2. Every token is associated with a vector that encodes its meaning.
3. The sequence of vectors passes through an attention block.
4. The vectors pass through a multi-layer perceptron or feed-forward layer.
5. The process alternates repeatedly between attention blocks and multi-layer perceptron blocks.
6. At the end, the essential meaning of the passage is intended to be encoded in the final vector.
7. An operation on that final vector produces a probability distribution over all possible next tokens.

There are also normalization steps between these operations.

# Tokens

Tokens are the small pieces into which an input is divided.

For text, tokens tend to be:

- Words.
- Pieces of words.
- Punctuation.
- Other common character combinations.

For images, tokens can be small patches of an image. For sound, they can be small chunks of sound.

Although tokens are not always full words, the video sometimes treats them as words to make examples easier to understand.

# Token Vectors

Each token is associated with a vector, meaning a list of numbers, intended to encode the meaning of that token.

If these vectors are treated as coordinates in a very high-dimensional space, words with similar meanings tend to have vectors that are close together.

Whenever the word *meaning* is used, that meaning is encoded entirely in the entries of these vectors.

# Attention

## Explanation for a 5–10-Year-Old Kid

Imagine reading this sentence:

> The dog chased the ball because it was rolling.

When you read the word *it*, your brain looks back and understands that *it* means the ball. You pay more attention to *ball* than to the other words.

A transformer does something similar. Each word looks at the other words and decides which ones are important for understanding it. It gives the important words more attention and uses information from them.

It is like every word asking the class, “Who can help me understand what I mean?” The useful words answer loudly, while the less useful words answer quietly.

## Explanation for a School Student

Words can have different meanings depending on the words around them. Consider:

> The bat flew out of the cave.

> He hit the ball with a bat.

The meaning of *bat* changes because of nearby words such as *flew*, *cave*, *hit*, and *ball*.

Attention is the mechanism that lets a transformer compare every token with other relevant tokens. It assigns an attention score to each connection. A high score means one token should strongly influence another; a low score means its information is less relevant.

The model uses these scores as weights. It combines more information from tokens with high attention scores and less information from tokens with low scores. This produces a new vector for each token that represents not only the token itself but also its context.

## Explanation for a Second-Year Computer Science Undergraduate

Attention is a data-dependent weighted-sum operation. Unlike a fixed neural-network layer that applies the same learned weights to every input, attention calculates its weights dynamically from the current token vectors.

For every input token vector $x_i$, the model creates three vectors using learned matrices:

$$
q_i=W_Qx_i,\qquad k_i=W_Kx_i,\qquad v_i=W_Vx_i
$$

- The **query** $q_i$ represents what token $i$ is looking for.
- The **key** $k_j$ represents what token $j$ offers or how it can be matched.
- The **value** $v_j$ contains the information token $j$ can contribute.

The relevance of token $j$ to token $i$ is measured using their query-key dot product:

$$
s_{ij}=q_i\cdot k_j
$$

A larger dot product means the query and key align more strongly. The scores are divided by $\sqrt{d_k}$ for numerical stability and passed through softmax:

$$
\alpha_{ij}
=\frac{\exp\left(s_{ij}/\sqrt{d_k}\right)}
{\sum_m\exp\left(s_{im}/\sqrt{d_k}\right)}
$$

The numbers $\alpha_{ij}$ are attention weights. They are positive and sum to 1 for each query. The output for token $i$ is the weighted sum of all value vectors:

$$
y_i=\sum_j\alpha_{ij}v_j
$$

In matrix form, for an input matrix $X$:

$$
Q=XW_Q,\qquad K=XW_K,\qquad V=XW_V
$$

$$
\operatorname{Attention}(Q,K,V)
=\operatorname{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
$$

Therefore, every output token vector is a context-dependent mixture of value vectors from relevant tokens.

In a text-generating transformer, a causal mask prevents a token from attending to future tokens. This preserves next-token prediction: a position may use only itself and earlier positions.

# Attention Blocks

An attention block allows token vectors to communicate, pass information back and forth, and update their values.

Attention determines:

- Which words in the context are relevant to other words.
- Which token meanings should be updated.
- How those meanings should be updated.

## Machine-Learning Model and Fashion Model Example

The word *model* has a different meaning in these two phrases:

- "A machine-learning model."
- "A fashion model."

The attention block uses the surrounding context to update the vector for *model* so that it represents the relevant meaning.

# Multi-Layer Perceptron or Feed-Forward Layer

After attention, the vectors pass through an operation called a **multi-layer perceptron** or **feed-forward layer**.

In this block, the vectors do not communicate with one another. They all pass through the same operation in parallel.

This operation can be thought of as asking a long list of questions about each vector and updating the vector according to the answers.

Both attention blocks and multi-layer perceptron blocks consist of a giant pile of matrix multiplications. Understanding these blocks requires learning how to read the underlying matrices.

# Repeated Transformer Blocks

The transformer repeatedly alternates between:

1. Attention blocks.
2. Multi-layer perceptron blocks.

The goal is for the final vector in the sequence to contain all the essential meaning needed from the passage to predict the next token.

# From a Predictor to a Chatbot

Early GPT-3 demonstrations autocompleted stories and essays from an initial snippet.

To turn a next-token predictor into a chatbot:

1. Begin with text that establishes a user interacting with a helpful AI assistant. This is the **system prompt**.
2. Add the user's question or prompt as the first part of the dialogue.
3. Ask the model to predict what the helpful AI assistant would say in response.

An additional training step is required to make this work well, but this is the high-level idea.

# Machine Learning

Machine learning describes models in which data is used to determine how the model behaves.

Examples include functions that:

- Take an image and produce a label describing it.
- Take a passage of text and predict the next word.
- Perform another task requiring intuition and pattern recognition.

Instead of explicitly writing a procedure for the task in code, machine learning uses a flexible structure with tunable parameters—like knobs and dials. Many examples of the correct output for a given input are used to adjust those parameters until the model mimics the desired behavior.

# Linear Regression Example

Linear regression is a simple form of machine learning in which both the input and output are single numbers.

For example:

- Input: The square footage of a house.
- Output: The price of the house.

The goal is to find a line of best fit through the data so that future house prices can be predicted.

A line can be described using two continuous parameters:

- Its slope.
- Its y-intercept.

Linear regression determines these two parameters so that the line closely matches the data.

# Deep Learning

Deep-learning models are much more complicated than linear regression. GPT-3 has 175 billion parameters rather than two.

It is not guaranteed that a giant model with many parameters will work. It might grossly overfit the training data or be completely intractable to train.

Deep learning describes a class of models that have proven to scale remarkably well over the last couple of decades.

These models use the same training algorithm: **backpropagation**.

For backpropagation to work well at scale, deep-learning models must follow a specific format. Understanding this format explains many design choices in a transformer that might otherwise seem arbitrary.

# Tensors and Layers

The input to a deep-learning model must be formatted as an array of real numbers.

This array can be:

- A list of numbers.
- A two-dimensional array.
- A higher-dimensional array, generally called a **tensor**.

The input data is progressively transformed through distinct layers. Every layer is an array of real numbers, ending with a final layer considered to be the output.

For the text-processing model, the final layer is a list of numbers representing a probability distribution over every possible next token.

# Weights and Weighted Sums

In deep learning, model parameters are almost always called **weights**.

The only way these parameters interact with the data being processed is through weighted sums. Nonlinear functions are also used, but those functions do not depend on parameters.

For inputs $x_1,x_2,\ldots,x_n$ and weights $w_1,w_2,\ldots,w_n$, a weighted sum has the form:

$$
w_1x_1+w_2x_2+\cdots+w_nx_n
$$

or:

$$
\sum_{j=1}^{n}w_jx_j
$$

Rather than writing every weighted sum separately, deep-learning models package many weighted sums into matrix-vector products.

# Matrix-Vector Multiplication

Let a matrix of tunable weights be:

$$
\mathbf{W}=
\begin{bmatrix}
w_{11} & w_{12} & \cdots & w_{1n}\\
w_{21} & w_{22} & \cdots & w_{2n}\\
\vdots & \vdots & \ddots & \vdots\\
w_{m1} & w_{m2} & \cdots & w_{mn}
\end{bmatrix}
$$

and let the data vector be:

$$
\mathbf{x}=
\begin{bmatrix}
x_1\\x_2\\\vdots\\x_n
\end{bmatrix}
$$

Then:

$$
\mathbf{W}\mathbf{x}=
\begin{bmatrix}
w_{11}x_1+w_{12}x_2+\cdots+w_{1n}x_n\\
w_{21}x_1+w_{22}x_2+\cdots+w_{2n}x_n\\
\vdots\\
w_{m1}x_1+w_{m2}x_2+\cdots+w_{mn}x_n
\end{bmatrix}
$$

Every component of the output is a weighted sum.

It is conceptually cleaner to think about matrices filled with tunable parameters transforming vectors drawn from the data being processed.

# GPT-3 Weight Matrices

GPT-3's 175 billion weights are organized into just under 28,000 distinct matrices. These matrices fall into eight categories.

Almost all of the computation inside a tool such as ChatGPT takes the form of matrix-vector multiplication.

# Weights Versus Data

A sharp distinction must be made between the model's weights and the data being processed:

- **Weights:** The learned brains of the model. They are learned during training and determine how the model behaves.
- **Data:** The numbers encoding the specific input supplied during one run, such as a snippet of text.

# Vocabulary

The model has a predefined vocabulary: a list of all possible tokens.

For an initial simplified example, this can be imagined as a vocabulary of 50,000 words. GPT-3 specifically uses a vocabulary of 50,257 tokens.

# Embedding Matrix

The first matrix in the transformer is the **embedding matrix**, labeled $W_E$.

It contains one column for every token in the vocabulary. Each column determines the vector into which that token is converted.

Like all the model's matrices, the embedding matrix begins with random values. Its values are then learned from data.

Turning words into vectors was common in machine learning before transformers.

# Word Embeddings

Embedding a word means representing it as a vector, which can be viewed geometrically as a point in a high-dimensional space.

A list of three numbers can be visualized as a point in three-dimensional space, but word embeddings are much higher-dimensional. GPT-3's embeddings have 12,288 dimensions.

To visualize high-dimensional word embeddings, a three-dimensional slice can be chosen and the word vectors projected onto it.

During training, the model adjusts its weights and tends to settle on embeddings in which directions have semantic meaning.

# Similar Words in Embedding Space

When searching for embeddings closest to the embedding of *tower*, the nearby words have similar tower-like meanings.

The model used for this visualization is not a transformer, but it illustrates how directions in embedding space can carry semantic meaning.

# Woman, Man, King, and Queen Example

A classic word-embedding example compares these vector differences:

$$
\mathbf{woman}-\mathbf{man}
$$

and:

$$
\mathbf{queen}-\mathbf{king}
$$

These directions are similar.

If the word for a female monarch were unknown, one could calculate:

$$
\mathbf{king}+\left(\mathbf{woman}-\mathbf{man}\right)
$$

and search for the closest embedding.

The true embedding of *queen* is farther away than this classic example might suggest, presumably because *queen* is not used in the training data only as the feminine version of *king*. Family relationships illustrated the general idea more effectively.

The model appears to find it useful to encode gender information as a direction in the embedding space.

# Italy, Germany, Hitler, and Mussolini Example

The following vector operation produces a point close to the embedding of *Mussolini*:

$$
\mathbf{Italy}-\mathbf{Germany}+\mathbf{Hitler}
\approx\mathbf{Mussolini}
$$

It is as if the model learned directions associated with Italian-ness and with World War II Axis leaders.

# Germany, Japan, Sushi, and Bratwurst Example

In some models, the following operation ends close to the embedding of *bratwurst*:

$$
\mathbf{Germany}-\mathbf{Japan}+\mathbf{sushi}
\approx\mathbf{bratwurst}
$$

# Cat, Beast, and Monster Example

When finding nearest neighbors in the embedding space, *cat* appears close to both *beast* and *monster*.

# Dot Product

The dot product measures how well two vectors align.

For vectors:

$$
\mathbf{u}=
\begin{bmatrix}u_1\\u_2\\\vdots\\u_n\end{bmatrix},
\qquad
\mathbf{v}=
\begin{bmatrix}v_1\\v_2\\\vdots\\v_n\end{bmatrix}
$$

their dot product is calculated by multiplying corresponding components and adding the results:

$$
\mathbf{u}\cdot\mathbf{v}
=u_1v_1+u_2v_2+\cdots+u_nv_n
=\sum_{i=1}^{n}u_iv_i
$$

This fits deep learning well because the computation is a weighted sum.

Geometrically:

- The dot product is positive when the vectors point in similar directions.
- It is zero when the vectors are perpendicular.
- It is negative when the vectors point in opposite directions.

# Plurality Direction Example

The vector difference:

$$
\mathbf{cats}-\mathbf{cat}
$$

might represent a plurality direction.

To test this, its dot product can be computed against the embeddings of singular nouns and compared with its dot product against the corresponding plural nouns.

The plural nouns consistently produce higher values than the singular nouns, suggesting that they align more strongly with the plurality direction.

Taking the same dot product with the embeddings of *one*, *two*, *three*, and so on produces increasing values. This acts like a quantitative measure of how plural the model finds a word.

# GPT-3 Embedding Parameters

GPT-3 has:

- A vocabulary size of 50,257 tokens.
- An embedding dimension of 12,288.

The embedding matrix therefore has this shape:

$$
W_E\in\mathbb{R}^{12{,}288\times50{,}257}
$$

Its parameter count is:

$$
12{,}288\times50{,}257=617{,}558{,}016
$$

This is about 617 million weights and is the first collection of weights in the model.

# Context-Rich Embeddings

Vectors in a transformer should not be thought of as representing only individual words.

They also encode the word's position, and they have the capacity to absorb context.

## Contextual King Example

A vector that begins as the embedding of *king* may be progressively pulled by the network's blocks until it points in a more specific direction. That final direction might encode that:

- The king lived in Scotland.
- He gained his position after murdering the previous king.
- He is being described in Shakespearean language.

A word's meaning is informed by its surroundings, including context from far away in the passage. A next-token prediction model therefore needs to incorporate context efficiently.

At the first step, every token vector is simply selected from the embedding matrix. It initially encodes only one token without information from neighboring tokens.

The main goal of the network is to let each vector absorb a meaning richer and more specific than an individual word could represent.

# Context Size

The network can process only a fixed number of vectors at one time. This is called its **context size**.

GPT-3 was trained with a context size of 2,048. The data flowing through the network is therefore an array of 2,048 columns, with each column having 12,288 dimensions.

The array has the shape:

$$
12{,}288\times2{,}048
$$

The context size limits how much text a transformer can use when predicting the next word.

## Long Chatbot Conversation Situation

Early versions of ChatGPT could appear to lose the thread during a long conversation because the context size limited how much earlier text the transformer could incorporate into its next prediction.

# Predicting the Next Token

The desired output is a probability distribution over all tokens that might come next.

This is produced in two steps:

1. Use a matrix to map the final context vector to one value for every token in the vocabulary.
2. Use softmax to normalize those values into a probability distribution.

## Professor Snape Example

Suppose the last word is *Professor*, while the context contains:

- *Harry Potter*.
- *Least favorite teacher* immediately before *Professor*.

A well-trained network with knowledge of Harry Potter would presumably assign a high value to *Snape* as the next word.

# Why the Final Vector Is Used

At prediction time, only the last vector is used even though thousands of other context-rich vectors are present in the final layer.

During training, it is more efficient to use every vector in the final layer simultaneously to predict the token that comes immediately after it.

# Unembedding Matrix

The **unembedding matrix** is labeled $W_U$.

It maps the final embedding vector to a list containing one value for every token in the vocabulary.

The unembedding matrix has:

- One row for every token in the vocabulary.
- One element in each row for every embedding dimension.

Its shape is:

$$
W_U\in\mathbb{R}^{50{,}257\times12{,}288}
$$

If the final vector is:

$$
\mathbf{x}_{\text{last}}\in\mathbb{R}^{12{,}288}
$$

the raw output is:

$$
\mathbf{z}=W_U\mathbf{x}_{\text{last}}
$$

where:

$$
\mathbf{z}\in\mathbb{R}^{50{,}257}
$$

The values in $W_U$ begin randomly and are learned during training.

Its parameter count is:

$$
50{,}257\times12{,}288=617{,}558{,}016
$$

This adds another 617 million parameters. Together, the embedding and unembedding matrices contain:

$$
617{,}558{,}016+617{,}558{,}016
=1{,}235{,}116{,}032
$$

This is a little over one billion parameters—a small but not insignificant fraction of GPT-3's 175 billion.

# Logits

The raw, unnormalized outputs produced by multiplying the final vector by the unembedding matrix are called **logits**.

For the next-token prediction, every component of $\mathbf{z}$ is the logit associated with one possible token.

These values may be negative, larger than 1, and they almost certainly do not add up to 1.

# Probability Distributions

For a sequence of numbers to represent a probability distribution:

- Every value must be between 0 and 1.
- All the values must add up to 1.

Matrix-vector multiplication does not naturally produce values satisfying these conditions.

# Softmax

Softmax is the standard way to convert an arbitrary list of numbers into a valid probability distribution.

Larger input values become probabilities closer to 1, while smaller input values become probabilities closer to 0.

For logits $z_1,z_2,\ldots,z_n$, softmax is:

$$
\operatorname{softmax}(\mathbf{z})_i
=\frac{e^{z_i}}{\sum_{j=1}^{n}e^{z_j}}
$$

It works in two stages:

1. Raise $e$ to the power of every input value, producing positive values.
2. Divide every positive value by their total sum.

The resulting values are positive and add up to 1:

$$
\sum_{i=1}^{n}\operatorname{softmax}(\mathbf{z})_i=1
$$

If one input is meaningfully larger than all the others, its output dominates the distribution, so sampling will almost certainly select it.

Softmax is softer than simply choosing the maximum. When several inputs are similarly large, they all receive meaningful probability, and the distribution changes continuously as the inputs change.

# Temperature

When a language model uses softmax to choose its next token, a constant $T$ can be added to the denominator of the exponents. This is called the **temperature**, because it resembles the role of temperature in some thermodynamics equations.

Temperature-adjusted softmax is:

$$
P_i=\frac{e^{z_i/T}}{\sum_{j=1}^{n}e^{z_j/T}}
$$

- A larger $T$ gives more weight to lower logits and makes the distribution more uniform.
- A smaller $T$ makes larger logits dominate more aggressively.
- At the extreme, temperature 0 places all weight on the maximum value.

# Story Generation at Different Temperatures

GPT-3 was given the seed text:

> Once upon a time there was A

At temperature 0, the model always selected the most predictable word. The resulting story was a trite derivative of *Goldilocks*.

At a higher temperature, the model could choose less likely words. The story began more originally with a young web artist from South Korea, but quickly degenerated into nonsense.

Higher temperature can produce more originality, but it also introduces more risk.

# API Temperature Constraint

The API discussed in the video does not allow temperatures higher than 2. This is not a mathematical restriction; it is an arbitrary constraint intended to stop the tool from appearing to generate excessively nonsensical output.

For the animation, the model returned the 20 most probable next tokens, which appeared to be the maximum allowed. Their probabilities were then adjusted using an exponent of $1/5$.

# Foundations for Attention

Understanding attention becomes smoother with strong intuition for:

- Word embeddings.
- Softmax.
- Dot products as measures of similarity.
- Matrix multiplication.
- Matrices filled with tunable parameters.

The attention mechanism is a cornerstone of the modern boom in AI.

# Further Material Mentioned

The video mentions:

- A following chapter focused on attention blocks, generally considered the heart of a transformer.
- Later discussions of multi-layer perceptron blocks, training, and other skipped details.
- A broader mini-series about deep learning that can be watched out of order.
- A draft of the attention chapter made available to Patreon supporters for review before its public release.
