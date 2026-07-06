# Lecture Script: Introduction to Large Language Models
**Format:** Facilitator-facing live script | **Duration:** 110 minutes | **Level:** Beginner

---

## Session Flow at a Glance

| Block | Topic | Time |
|---|---|---|
| 1 | Why Does This Matter? | 10 min |
| 2 | LLM Fundamentals | 25 min |
| 3 | Transformer Architecture and Its Components | 40 min |
| 4 | Tokenization and Its Role in Text Processing | 25 min |
| 5 | Lecture Summary and Recap | 10 min |

---

## Block 1 — Why Does This Matter?

> 🎯 **Instructor Note:** This audience has built backends, databases, and secure APIs. This is their first encounter with LLMs and transformer models. The hook should connect LLMs to something they already do — building applications — and make clear why understanding the internals matters beyond just calling an API. Open with a practical problem that understanding transforms solves. Wait after the opening question.

**[Script:]**

"You have built APIs that return structured data — a user object, a product list, a filtered result set. Now you need to build something different. Your application needs to generate text that is relevant to a user's context, coherent across multiple sentences, and semantically appropriate to the situation.

You could prompt-engineer your way through it, tossing text at a commercial API and hoping it works. That works for some applications. But the moment you need to optimize for cost, latency, privacy, or specificity to your domain, you need to understand what is happening inside the model. Why does one prompt produce better results than another? Why do certain architectures handle long documents better? Why is the cost per token what it is? Why does one model hallucinate and another does not?

These questions only make sense once you understand the foundations: what an LLM actually is, how the transformer architecture works, and what tokenization does. At the end of this session you will not be able to build an LLM from scratch — that takes months of research and massive computational resources. But you will understand the mental model well enough to read a research paper, evaluate different models for your use case, debug when something goes wrong, and make architectural decisions about whether to fine-tune, build a retrieval system, or prompt-engineer your way through a problem.

An LLM is not magic. It is a specific mathematical architecture trained on specific data, with specific behaviors and specific limitations. Understanding what is in the box means you stop treating it as a black box."

---

## Block 2 — LLM Fundamentals

### 2A — What an LLM Is

**[Script:]**

"An LLM — Large Language Model — is a neural network trained on enormous amounts of text data to predict the next word in a sequence. That is the entire core capability. Given some text, the model computes a probability distribution over the vocabulary of possible next words, and picks one. Then it uses that word as input and predicts the next one. Repeat until you get a response of useful length.

That simple capability — predicting the next word — is powerful enough to generate coherent essays, answer questions, write code, and solve problems. It is not intelligence in any abstract sense; it is statistical pattern matching at a scale and on data so large that the patterns become capable of representing meaningful reasoning."

> 🎯 **Instructor Note:** Write this core idea on the board and keep it visible through the entire session.

```
LLM capability:
Given text input, predict the probability of each next word.
Pick a word (usually the highest probability, or sampled probabilistically).
Use that word as new input.
Repeat until done.

That is the entire mechanism. Everything else is engineering around it.
```

**[Script:]**

"There are four key points that define an LLM.

First, it is Large — trained on billions or trillions of words. Smaller models on smaller datasets behave very differently. The scale is where the emergent capabilities come from.

Second, it is specifically a Language model. It is trained on text, not images, not sound, not any other modality. There are multimodal models, but the core is language.

Third, it is Generative — it generates new text, word by word, rather than just classifying or summarizing existing text.

Fourth, it is a Neural Network — it uses layers of mathematical transformations, learned through training on data, not hard-coded rules. The structure of that neural network is the transformer architecture we will cover next."

---

### 2B — How LLMs Are Trained

**[Script:]**

"LLMs are trained using a process called next-token prediction. You take a massive corpus of text — billions of examples from books, websites, code repositories, everything. You break each example into tokens — chunks of text, typically a few characters to a word. You mask one token and train the model to predict it from the context around it.

Do this billions of times across billions of tokens, and the model gradually learns patterns. Patterns about grammar, patterns about facts, patterns about reasoning. The model never explicitly learns a rule like 'a noun usually follows an adjective'. But by seeing that pattern repeated millions of times in the training data, the model learns to assign higher probability to that outcome."

> 🎯 **Instructor Note:** Ask: "What happens if the training data contains a lot of false information?" Answer: the model learns false information as statistical patterns. This is why LLMs hallucinate — they are learning what is statistically likely given the data they saw, not what is actually true. A pattern that is false but consistent in the data still gets learned.

**[Script:]**

"Once trained on this enormous dataset, the model can be fine-tuned on smaller, more specific datasets for particular tasks. ChatGPT was fine-tuned on human feedback, adjusting the model's behavior to follow instructions and be helpful. Smaller, specialized models are fine-tuned on code, or medical text, or legal contracts. The fine-tuning layer is where much of the 'personality' or specificity of a model comes from."

**Recap of Block 2 before moving on:**

- An LLM is a neural network trained to predict the next word given preceding text
- At generation time, it outputs a probability distribution over possible next words and samples one, then repeats
- LLMs are large because they are trained on billions of words of text data; scale is where capabilities emerge
- Training is next-token prediction on a massive corpus; fine-tuning adapts a trained model to specific tasks or styles
- LLMs learn statistical patterns from their training data, not abstract rules; false patterns in the data produce hallucinations

---

## Block 3 — Transformer Architecture and Its Components

### 3A — Why Transformers Exist

**[Script:]**

"Before transformers, the standard architecture for processing sequences was recurrent neural networks — RNNs. An RNN processes text word by word, maintaining hidden state as it goes. The problem: it processes sequentially, so each word must wait for the previous word to be processed. On a text with a thousand words, you cannot parallelize; you have to do them one at a time.

Transformers solved this with a revolutionary insight: you can process all the words at once if you give each word the ability to attend to every other word in parallel. Attention is the key mechanism."

> 🎯 **Instructor Note:** Draw the comparison on the board.

```
RNN: Word 1 → Word 2 → Word 3 → ... (sequential, slow)
Transformer: All words process in parallel,
            each word can attend to all others (parallel, fast)
```

**[Script:]**

"This parallelism is why transformers work on modern GPUs. It is also why they can handle longer documents than RNNs — with some limitations we will address. The transformer architecture has become the standard because of this speed advantage."

---

### 3B — The Transformer Architecture Overview

**[Script:]**

"A transformer has six main components. Let me name them, then we will look at each one."

> 🎯 **Instructor Note:** Write these on the board as a list learners will reference.

```
1. Input Embedding        — convert tokens to vectors
2. Positional Encoding    — add information about token positions
3. Self-Attention        — each token attends to all others
4. Feed-Forward Network  — process each token through dense layers
5. Layer Normalization   — normalize activations
6. Output Layer          — convert final vectors back to probabilities
```

**[Script:]**

"A full transformer stacks multiple copies of steps 3 and 4 — multiple attention layers followed by feed-forward layers, called a 'block' or 'layer'. Modern models stack dozens or hundreds of these blocks.

Let us understand what each component does and why it is there."

---

### 3C — Input Embedding and Positional Encoding

**[Script:]**

"Input embedding converts tokens into vectors — lists of numbers that the neural network can process. A token like 'cat' becomes a vector of 768 or 1024 numbers, depending on the model size. These vectors are learned during training; the embedding layer learns to represent tokens in a way that is useful for predicting the next token.

Positional encoding adds information about where each token is in the sequence. Without it, the transformer would treat 'The cat sat on the mat' identically to 'mat the on sat cat The' — the order would not matter. Positional encoding adds a signal to each embedding that encodes its position, so the model knows where each word is."

> 🎯 **Instructor Note:** Draw the embedding process on the board.

```
Token: "cat"
    ↓ embedding lookup
Vector: [0.2, -0.5, 0.1, ..., 0.7] (768 dimensions)
    ↓ add positional encoding
Output: [0.2, -0.4, 0.2, ..., 0.6] (positional info added)
```

---

### 3D — Self-Attention

**[Script:]**

"Self-attention is the core innovation of transformers. It answers the question: for each token, how much should it attend to each other token in the sequence?

The mechanism: each token creates three representations — a query ('what information am I looking for'), a key ('what information do I have'), and a value ('what do I contribute'). The attention weight between two tokens is how well the query of one matches the key of the other. High match means high attention. The output is a weighted sum of all values, weighted by the attention scores.

This happens in parallel for all pairs of tokens, which is why transformers are fast."

> 🎯 **Instructor Note:** Draw the attention mechanism step by step on the board.

```
Token A wants to look at Token B.
Query_A · Key_B = attention_score_AB  (dot product, high if match)

For Token A, compute attention to every other token.
Softmax(attention_scores) = normalized weights (sum to 1)

Output_A = sum of (Value_B × weight_AB) for all tokens B
```

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If a token has very high attention weight on one other token, does it mean it is ignoring the rest?" Not necessarily — the weights sum to 1, so if attention is spread across many tokens, each weight is small. If concentrated on one token, that one gets a large weight and others get small weights. The model learns which tokens to focus on during training.

**Demo 1 — Attention in a simple sentence (whiteboard-friendly)**

```
Sentence: "The bank manager closed the account"

Word: "bank"
  Attends most to: "manager" (0.4), "closed" (0.3), "account" (0.2)
  Attends least to: "the" (0.05)

Word: "closed"
  Attends most to: "manager" (0.3), "bank" (0.25), "account" (0.35)
  Attends least to: "the" (0.05)

The model learns that "bank" should attend to "manager" and "account"
to understand context.
```

**[Script:]**

"In training, the model learns these attention patterns. 'bank' learns to focus on 'manager' because in the training data, 'bank manager' appears often together. It learns to focus on 'account' because 'closed the account' is a common phrase.

This is how transformers capture long-range dependencies. A word near the end of a long sentence can attend to words near the beginning, learning that they are relevant. An RNN would have to pass information through every intermediate word, losing it along the way."

> 🎯 **Instructor Note:** Pause here and ask: "Why is attention parallel, whereas an RNN is sequential?" Answer: attention computes weights between all pairs at once using matrix operations, which GPUs can parallelize. An RNN has to process word by word because each word depends on the hidden state from the previous word, which cannot be computed until the previous step finishes. This is why transformers can process a thousand-word document in one forward pass; an RNN would take a thousand sequential steps.

---

### 3E — Feed-Forward and Output

**[Script:]**

"After attention, each token passes through a feed-forward network — a small neural network applied independently to each token. This adds nonlinearity and lets the model compute more complex functions.

The transformer repeats attention and feed-forward blocks multiple times, each adding more layers of processing. After all blocks, a final linear layer converts the output vectors back to probabilities over the vocabulary. The highest-probability word is the model's prediction for the next token."

**Recap of Block 3 before moving on:**

- Transformers use attention instead of recurrence, allowing parallel processing of entire sequences
- Input embedding converts tokens to vectors; positional encoding adds information about token positions
- Self-attention computes how much each token should attend to every other token
- Attention is computed as a weighted sum of values, with weights based on how well queries match keys
- Feed-forward networks and layer normalization add depth and nonlinearity
- Multiple transformer blocks stack these components, creating a deep network

---

## Block 4 — Tokenization and Its Role in Text Processing

### 4A — What Tokenization Is and Why It Matters

**[Script:]**

"Tokenization is the process of converting text into tokens — discrete chunks that the model processes. A token might be a word, a subword, or even a single character, depending on the tokenization scheme.

Why not just feed the model raw text? Because neural networks work with fixed-size inputs. They need to know the vocabulary size ahead of time. Tokenization creates that vocabulary and converts arbitrary text into a sequence of token IDs."

> 🎯 **Instructor Note:** Ask: "If I tokenize 'The cat sat' and get [1, 2, 3] and tokenize 'The dog ran' and get [1, 4, 5], what do the repeated 1s tell us?" Answer: both sentences start with 'The', which got assigned token ID 1. Tokenization maps words to integer IDs consistently.

**[Script:]**

"But tokenization has hidden importance. The tokenizer determines the vocabulary size, which affects model size — a larger vocabulary means more parameters in the embedding layer. It determines how text is split — do you split on spaces, on characters, on subwords? This affects how the model sees language."

---

### 4B — Tokenization Schemes

**[Script:]**

"There are several approaches to tokenization, each with tradeoffs.

Character-level tokenization: each character is a token. Simple, handles any text, but sequences become very long and the model must learn to combine characters into words.

Word-level tokenization: each word is a token. Fast sequences, but you need to handle unknown words. A word not in the training data cannot be represented.

Subword tokenization: words are split into smaller pieces if they are rare or unseen. Common pieces like 'ing', 'tion', 'un' become tokens. Unknown words can be built from subwords. This balances the tradeoffs."

> 🎯 **Instructor Note:** Draw examples on the board.

```
Character-level:
"unhappy" → ['u', 'n', 'h', 'a', 'p', 'p', 'y']

Word-level:
"unhappy" → ['unhappy']

Subword (BPE):
"unhappy" → ['un', 'happy']
```

**[Script:]**

"Most modern LLMs use subword tokenization, specifically Byte Pair Encoding or SentencePiece. The vocabulary is learned from the training data — frequent sequences of bytes or characters become tokens, and this process repeats to build a vocabulary of 30k-50k tokens."

**Predict before running: What will happen?**

> 🎯 **Instructor Note:** Ask: "If my tokenizer's vocabulary is 50,000 tokens, and I tokenize a sentence with 10 words, how many tokens do I get?" Answer: it depends. If each word is a known token, 10. If some words are split into subwords, more than 10. For example, "unbelievably" might be ['un', 'believ', 'ably'] = 3 tokens instead of 1 word.

**Demo 2 — Tokenization example (whiteboard-friendly)**

```
Sentence: "The model predicts the next token."

Tokenizer vocabulary includes:
  "The", "model", "predicts", "next", "token", ".", "<unk>"

Tokenization:
"The"       → token ID 1
"model"     → token ID 2
"predicts"  → token ID 3
"the"       → token ID 1 (lowercase, same as "The" after normalization)
"next"      → token ID 4
"token"     → token ID 5
"."         → token ID 6

Output: [1, 2, 3, 1, 4, 5, 6]

This sequence of 7 token IDs is what the transformer processes.
```

**[Script:]**

"The tokenizer is fixed for a given model. GPT models use a specific tokenizer with a specific vocabulary. When you send text to the API, the tokenizer splits it. The model processes the tokens. The output logits are probabilities over the same vocabulary. The logits are decoded back to text using the same tokenizer."

---

### 4C — Why Tokenization Affects Model Behavior

**[Script:]**

"Tokenization affects how the model sees language in surprising ways. Consider the word 'unhappy'. In one tokenizer it might be a single token. In another it might be ['un', 'happy']. If it is split, the model must learn the meaning from two separate tokens and the attention patterns between them. The first tokenizer 'knows' unhappy is a word; the second must learn it from patterns.

This affects model behavior. Some models handle code poorly if their tokenizer treats common operators as multiple tokens. Some handle non-English languages poorly if their vocabulary is skewed toward English.

Cost is affected too. If you pay per token and one tokenizer splits text into more tokens, the same text costs more. A model with a 30k-token vocabulary might tokenize English more efficiently than one with a 50k-token vocabulary."

> 🎯 **Instructor Note:** Ask: "If a model's tokenizer includes the token 'ing', and the word 'running' is one token, but 'running' is not in the vocabulary and must be split into ['run', 'ning'], does the model see them differently?" Answer: yes, substantially. One token vs two changes how much compute is needed (two attention heads instead of one), how many parameters are dedicated to it, and how easily the model can learn its semantics. This is why tokenization vocabulary design matters.

**Recap of Block 4 before moving on:**

- Tokenization converts text into discrete tokens, which the model processes as integer IDs
- Character-level tokenization is flexible but creates long sequences; word-level is efficient but cannot handle unknown words; subword tokenization balances both
- Most modern LLMs use subword tokenization with vocabularies of 30k-50k tokens
- Tokenization affects model size, sequence length, training efficiency, inference cost, and semantic learning
- The same tokenizer is used both at training and inference time; mismatches cause errors

---

## Block 5 — Lecture Summary

> 🎯 **Instructor Note:** Deliver as active recall. Ask before confirming. "What is the core capability of an LLM? Why do transformers use attention? What does positional encoding do? Why does tokenization matter beyond just converting text to numbers?"

**LLM Fundamentals — Introduction to Large Language Models**

- An LLM is a neural network trained to predict the next word given preceding text, by learning statistical patterns from massive amounts of text data
- LLMs are large because scale is where emergent capabilities come from; billions of parameters trained on trillions of tokens
- Training is next-token prediction on a massive corpus; fine-tuning on smaller, task-specific data adapts the model to particular use cases
- LLMs generate text by iteratively sampling the next word, using that as input, and repeating; they learn patterns, not rules, so they can hallucinate false information present in training data

**Transformer Architecture and Its Components**

- Transformers use attention instead of recurrence, allowing parallel processing of entire sequences; each token attends to every other token
- Input embedding converts tokens to vectors; positional encoding adds information about token positions so order matters
- Self-attention computes how much each token should attend to every other token based on query-key matching, allowing the model to learn long-range dependencies
- Feed-forward networks add nonlinearity and depth; layer normalization stabilizes training
- Multiple transformer blocks stack these components; the output layer converts final vectors to probabilities over the vocabulary

**Tokenization and Its Role in Text Processing**

- Tokenization converts text to token IDs, creating the vocabulary the model works with
- Character-level tokenization is flexible but slow; word-level is fast but cannot handle unknown words; subword tokenization (BPE, SentencePiece) balances both
- Most modern LLMs use subword tokenization with vocabularies of 30k-50k tokens learned from training data
- Tokenization affects model size, sequence length, cost, and how the model learns semantics; the same tokenizer is used at training and inference

**Why All of This Matters Together**

- Understanding LLM fundamentals, transformer architecture, and tokenization means you can read model documentation and understand why one model works better than another for your use case, debug when behavior is unexpected, estimate computational and cost requirements, and make architectural decisions about when to use an API versus fine-tune versus build a retrieval system around a base model — the difference between treating LLMs as a black box and understanding the box well enough to use it deliberately and effectively

---

*End of script.*