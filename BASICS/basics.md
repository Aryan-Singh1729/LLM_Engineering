# Large Language Models

A large language model is a sophisticated mathematical function that predicts what word comes next for any piece of text.

Instead of predicting one word with certainty, it assigns a probability to all possible next words.

## Movie Script Situation

Imagine you happen across a short movie script that describes a scene between a person and their AI assistant. The script has what the person asks the AI, but the AI's response has been torn off.

Suppose you also have a powerful magical machine that can take any text and provide a sensible prediction of what word comes next. You could finish the script by:

1. Feeding what you have into the machine.
2. Seeing what it would predict to start the AI's answer.
3. Repeating this over and over with a growing script, completing the dialogue.

When you interact with a chatbot, this is exactly what's happening.

# Chatbots

To build a chatbot:

1. Lay out some text that describes an interaction between a user and a hypothetical AI assistant.
2. Add whatever the user types in as the first part of the interaction.
3. Have the model repeatedly predict the next word that such a hypothetical AI assistant would say in response.
4. Present that response to the user.

The output tends to look a lot more natural if you allow the model to select less likely words along the way at random. Even though the model itself is deterministic, a given prompt typically gives a different answer each time it is run.

# Context

Context is all the information available to the model when it predicts the next token. It can include the system instructions, the user's prompt, earlier messages in the conversation, and any documents or data included with the request.

The model uses this context to understand what words mean in the current situation. For example, the word "bank" could mean a financial institution or the side of a river. The surrounding words help the model decide which meaning is relevant.

The model does not remember information in the same way a person does. During a request, it can directly use only the information included in its current context and patterns stored in its trained parameters.

# Training Data

Models learn how to make predictions by processing an enormous amount of text, typically pulled from the internet.

## GPT-3 Training Data Example

For a standard human to read the amount of text that was used to train GPT-3, if they read non-stop 24-7, it would take over 2,600 years. Larger models since then train on much, much more.

# Parameters or Weights

You can think of training a little bit like tuning the dials on a big machine.

The way that a language model behaves is entirely determined by many different continuous values, usually called parameters or weights. Changing those parameters changes the probabilities that the model gives for the next word on a given input.

What puts the "large" in large language model is how these models can have hundreds of billions of parameters.

No human ever deliberately sets those parameters. Instead, they begin at random, meaning the model just outputs gibberish, but they are repeatedly refined based on many example pieces of text.

Technically, parameters are the numerical values that the model learns during training. **Weights** are the main type of parameter: they control how strongly one piece of information affects another. A model can also have **biases**, which shift a calculation up or down.

During a forward pass, the model combines its input numbers with these weights to produce a prediction. Training measures the prediction error using a **loss function**. Backpropagation calculates how much each parameter contributed to that error, and an optimizer changes the parameters slightly to reduce future errors.

A single parameter does not usually store a complete fact. Knowledge and language patterns are generally distributed across many parameters working together.

# Training Examples

One training example could be just a handful of words, or it could be thousands.

In either case, the process is:

1. Pass all but the last word from the example into the model.
2. Compare the prediction that the model makes with the true last word from the example.
3. Use backpropagation to tweak all of the parameters so the model becomes a little more likely to choose the true last word and a little less likely to choose all the others.

When this is done for many, many trillions of examples, the model starts to give more accurate predictions on the training data. It also starts to make more reasonable predictions on text that it has never seen before.

# Backpropagation

Backpropagation is an algorithm used to tweak all of the parameters in such a way that the model becomes a little more likely to choose the true last word and a little less likely to choose all the others.

# Scale of Computation

Given the huge number of parameters and the enormous amount of training data, the scale of computation involved in training a large language model is mind-boggling.

## One Billion Operations per Second Situation

Imagine that you could perform one billion additions and multiplications every single second.

How long would it take to do all of the operations involved in training the largest language models? It would not take a year or something like 10,000 years. It would take well over 100 million years.

# Pre-training

The whole process of training a model to autocomplete passages of text is called pre-training.

The goal of auto-completing a random passage of text from the internet is very different from the goal of being a good AI assistant.

# Reinforcement Learning with Human Feedback

To address the difference between auto-completing random internet text and being a good AI assistant, chatbots undergo another type of training, just as important, called reinforcement learning with human feedback.

Workers flag unhelpful or problematic predictions, and their corrections further change the model's parameters, making the model more likely to give predictions that users prefer.

# GPUs

The staggering amount of computation used in pre-training is only made possible by special computer chips optimized for running many operations in parallel, known as GPUs.

However, not all language models can be easily parallelized.

# Language Models Before 2017

Prior to 2017, most language models would process text one word at a time.

# Transformers

In 2017, a team of researchers at Google introduced a new model known as the transformer.

Transformers do not read text from the start to the finish. They soak it all in at once, in parallel.

The first step inside a transformer, and most other language models, is to associate each word with a long list of numbers.

The training process only works with continuous values, so language has to be encoded using numbers. Each list of numbers may somehow encode the meaning of the corresponding word.

What makes transformers unique is their reliance on a special operation known as attention.

Transformers typically also include a second type of operation known as a feed-forward neural network.

All of the data repeatedly flows through many different iterations of these two fundamental operations. As it does so, the hope is that each list of numbers is enriched to encode whatever information might be needed to make an accurate prediction of what word follows in the passage.

More technically, a transformer first converts tokens into vectors called **embeddings**. It also adds positional information so that the model knows the order of the tokens. These vectors then pass through a stack of transformer blocks.

Each transformer block mainly contains:

1. A self-attention layer, which mixes information between tokens.
2. A feed-forward neural network, which processes each token's updated representation.
3. Residual connections and layer normalization, which help information and gradients move reliably through a deep model.

After the final block, the last token's vector is converted into scores called **logits**, with one score for each possible next token. A softmax function turns those scores into probabilities.

# Attention

Attention gives all of the lists of numbers a chance to talk to one another and refine the meanings they encode based on the context around them, all done in parallel.

Technically, self-attention creates three vectors for every token:

- **Query:** what information this token is looking for.
- **Key:** what information this token can be matched by.
- **Value:** the information this token can provide.

The model compares a token's query with the keys of other tokens. A stronger match produces a higher attention score. After these scores are normalized, they are used to calculate a weighted combination of the value vectors. The result is a new representation containing information from the most relevant tokens.

Transformers use multiple attention heads. Each head can learn to focus on different relationships, such as nearby words, earlier references, or grammatical connections.

## Bank and Riverbank Example

The numbers encoding the word "bank" might be changed based on the context surrounding it to somehow encode the more specific notion of a riverbank.

# Feed-forward Neural Network

A feed-forward neural network gives the model extra capacity to store more patterns about language learned during training.

# Predicting the Next Word Inside a Transformer

At the end, one final function is performed on the last vector in the sequence. This vector has had a chance to be influenced by all the other context from the input text, as well as everything the model learned during training.

The function produces a prediction of the next word. The model's prediction looks like a probability for every possible next word.

# Emergent Behavior

Researchers design the framework for how each step works, but the specific behavior is an emergent phenomenon based on how hundreds of billions of parameters are tuned during training.

This makes it incredibly challenging to determine why the model makes the exact predictions that it does.

When large language model predictions are used to autocomplete a prompt, the words generated are uncannily fluent, fascinating, and even useful.

# Further Material Mentioned in the Video

For more details on how transformers and attention work, the video mentions:

- A deep learning series that visualizes and motivates the details of attention and all the other steps in a transformer.
- A talk given a couple of months earlier for the company TNG in Munich, posted on the creator's second channel.
- The creator sometimes prefers content made as a casual talk rather than as a produced video, but leaves it to the viewer to decide which feels like the better follow-on.
