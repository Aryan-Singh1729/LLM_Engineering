<style>
  @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600&display=swap');
  .notes-container {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    font-size: 1.15em;
    line-height: 1.65;
    font-weight: 300; /* Beautiful, clean, lightweight body text */
  }
  .notes-container h1, .notes-container h2, .notes-container h3, .notes-container h4 {
    font-weight: 500; /* Sleek, medium-weight headings instead of standard heavy bold */
  }
  .notes-container strong {
    font-weight: 500; /* Slightly bolder key terms for elegant contrast */
  }
</style>

<div class="notes-container">

# A. Base Models

*   **What they do:** They take a sequence of text data as input and simply **predict the most likely text token to come next**. They are **raw text-completion engines**.
*   **How they were inspired:** They represent the **foundational state** of LLM technology (like early GPT-3). They mimic **basic human predictive habits**, much like the **predictive text on your smartphone keyboard** that guesses the next word as you type.
*   **Production Trade-offs:** They are **completely unpolished and practically unusable for general interactive chat** applications. However, they are **highly valuable as a raw starting canvas** when you want to **fine-tune or train a model from scratch** to have a completely custom skill or behavioral construct.

<br>

# B. Chat / Instruct Variants

*   **What they do:** They are models **explicitly trained to follow human instructions** and converse in a conversational, structured, back-and-forth framework (`system`, `user`, and `assistant` prompts).
*   **How they were inspired:** Early developers were manually formatting their prompts with structured "Q:" and "A:" text sequences to force base models into answering questions. OpenAI realized they could bake this behavior directly into the model by training it on structured conversation data using **Reinforcement Learning from Human Feedback (RLHF)**, creating ChatGPT.
*   **Production Trade-offs:** They are **fast, cheap to run, and excellent for conversational user interfaces**. They also excel at generating **fluid, natural, and creative text** (like emails). On the downside, they **lack deep analytical reflection** and are **highly prone to logic or calculation errors** when faced with complex puzzles.

<br>

# C. Reasoning / Thinking Models

*   **What they do:** They are trained to generate an **explicit, internal thought process (a "thinking trace")** in the background before they deliver their final answer to the user.
*   **How they were inspired:** Users discovered a famous prompt engineering trick: adding the phrase *"Please think step-by-step"* to the end of a prompt drastically improved the model's accuracy on hard tasks. AI labs realized they could automate this by **hard-training models on millions of examples** that **explicitly output reasoning paths** before the conclusion.
*   **Production Trade-offs:** They **excel at hard problem-solving, math, coding, and logical tasks**. However, they are **significantly slower and much more expensive to run** because you have to pay for the generation costs of all those **hidden background "thinking tokens."** They can also sometimes sound **overly stiff, cold, and analytical**.

<br>

# D. Hybrid Models

*   **What they do:** These are **modern, adaptive models** (like Gemini 2.5 Pro or GPT-5) that can **dynamically scale their processing power** up or down on the fly depending on the difficulty of the prompt.
*   **How they were inspired:** They were created to **solve the inefficiency of reasoning models** (it is a waste of processing power and time to spend hundreds of "thinking tokens" on a simple greeting like "Hi"). Hybrid models were designed to **intelligently allocate a "reasoning budget"** only when a problem actually demands it.
*   **Production Trade-offs:** They offer the **ideal balance for general-purpose applications**, acting **fast for simple queries** and **thinking deeply for complex puzzles**. The main trade-off is that they **introduce unpredictability into your production pipeline**, as **latency and API token costs will vary wildly** from request to request depending on how much the model decides to "think."

<br>
<hr>
<br>

# Budget Forcing (the power of "Wait")

*   **Budget Forcing:** A technique from the **S1 paper** (January 2025) that manually controls a model's **inference-time compute** (how long it thinks) without needing to retrain it.

<br>

# The Mechanism ("The Wait Hack")

*   **Forced Continuation:** Reasoning models output thoughts inside `` `&lt;think&gt;` `` tags. When the model tries to finish thinking and output `` `&lt;/think&gt;` ``, the system backend **deletes that tag**.
*   **The Injection:** The system injects a single token—the word **`"Wait"`**—directly into the context window.
*   **The Reaction:** Because transformers predict text based on history, reading **`"Wait"`** tricks the model into entering a **state of self-doubt**. It shifts back into reasoning mode (*"Wait, let me rethink that..."*).

<br>

# Why It Works

*   **Error Correction:** It **breaks the model's momentum** down a flawed logical path, forcing it to **scan its previous reasoning for bugs**.
*   **Alternative Paths:** It pushes the model to **verify its initial conclusions** using secondary problem-solving methods.

<br>

# Key Trade-offs

*   **Test-Time Scaling:** It effectively makes **smaller models dramatically smarter at runtime** (e.g., matching giant proprietary models on math/coding benchmarks) just by giving them time to think.
*   **The Limits:** Accuracy plateaus after **5–6 forced `"Wait"` iterations**. It **heavily spikes API token costs** and risks filling up the model's maximum context window.

</div>
