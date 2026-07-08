# Attention

Attention is one of the key mechanisms inside a transformer, and transformers are one of the key technologies behind large language models and many other modern AI systems.

The transformer architecture first became famous through the 2017 paper **Attention Is All You Need**.

This chapter focuses on what the attention mechanism is meant to do before getting into the detailed matrix multiplications.

# Recap: What the Model Is Doing

The model being studied takes in text and predicts what word comes next.

The input text is first broken into small pieces called **tokens**. In real systems, tokens are often words, parts of words, punctuation, or other common character groups. For simpler examples, it is useful to pretend that tokens are always words.

The first step in a transformer is to associate each token with a high-dimensional vector called its **embedding**.

# Embeddings and Meaning

An embedding is a vector that represents a token inside a very high-dimensional space.

One of the most important ideas is that directions in this high-dimensional space can correspond to aspects of meaning.

For example, a certain direction might correspond to gender. Moving in that direction could take the embedding of a masculine noun toward the embedding of the corresponding feminine noun.

That is only one possible direction. Many other directions in the embedding space can correspond to many other features of meaning.

# The Goal of a Transformer

The aim of a transformer is to progressively adjust token embeddings.

At the beginning, each embedding mainly represents an individual token. After passing through the transformer, the embeddings should encode much richer contextual meaning.

In other words, the vector for a word should not merely represent the word in isolation. It should represent what that word means in this specific passage.

# Why Attention Can Be Confusing

Many people find the attention mechanism confusing at first.

This is normal because attention is not just a single intuitive operation. It is a learned mechanism that decides how information from surrounding tokens should influence each token's vector.

Before studying the computations, it helps to understand the kind of behavior attention is supposed to enable.

# Example: The Word Mole

Consider these three phrases:

- `American shrew mole`
- `one mole of carbon dioxide`
- `take a biopsy of the mole`

The word `mole` has a different meaning in each phrase:

- In the first phrase, it refers to an animal.
- In the second phrase, it refers to a scientific unit.
- In the third phrase, it refers to a spot on the skin.

After the first embedding step, however, the vector associated with `mole` would be the same in all three cases.

This happens because the initial token embedding is basically a lookup table. It does not yet know anything about the surrounding context.

# How Attention Helps

Attention gives the surrounding token embeddings a chance to pass information into the embedding for a given token.

For the word `mole`, a well-trained attention block can use the surrounding words to update the generic embedding into a more specific contextual meaning.

One way to picture this is:

1. The original embedding points toward the general meaning of `mole`.
2. The context suggests which meaning is intended.
3. The attention block calculates what should be added to the original embedding.
4. The updated vector moves toward the meaning that fits the context.

So the embedding for `mole` should shift differently depending on whether the context is about animals, chemistry, or medicine.

# Example: The Word Tower

Consider the embedding for the word `tower`.

By itself, this vector might represent a generic idea of a tall structure. It could be associated with other large, tall nouns.

If the word `tower` is immediately preceded by `Eiffel`, the model should update the vector so that it more specifically represents the **Eiffel Tower**.

That updated vector might become more correlated with ideas such as:

- Paris.
- France.
- Steel.
- Famous landmarks.

If the word `miniature` also appears before it, the vector should be updated again.

Now the meaning should no longer strongly point toward something large and tall. The phrase `miniature Eiffel tower` refers to a small object, even though the original word `tower` usually suggests something big.

# Moving Information Between Tokens

Attention is not only about refining the meaning of individual words.

More generally, an attention block allows the model to move information encoded in one embedding into another embedding.

This information transfer can happen:

- Between nearby tokens.
- Between tokens that are far apart.
- With information richer than the meaning of a single word.

This is one reason attention is so powerful. It lets different parts of the input communicate with each other.

# The Final Vector Matters

After all the vectors flow through the transformer, including many attention blocks and other layers, the prediction for the next token is computed from the final vector in the sequence.

For example, suppose the input text is almost an entire mystery novel, ending with:

`therefore the murderer was`

To predict the next word accurately, the final vector, which began as the embedding for the word `was`, must contain far more than the meaning of that one word.

It needs to have been updated by the network so that it somehow represents the relevant information from the full context window.

That might include:

- Characters.
- Motives.
- Clues.
- Events from earlier in the story.
- The structure of the sentence being completed.

# Key Idea

Attention lets a transformer update each token's embedding based on the surrounding context.

The initial embedding gives the model a generic meaning for each token. Attention helps transform that generic meaning into a contextual meaning by moving relevant information between embeddings.

This is how a transformer can take words that start as isolated lookup-table vectors and gradually turn them into rich representations of what those words mean in the full passage.
