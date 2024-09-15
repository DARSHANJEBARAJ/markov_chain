# Markov Chain Text Generation

## Project Overview

This project implements a **Markov Chain-based text generation algorithm**. The model generates text by predicting the next word (or character) based on the current word(s) or character(s). It’s a simple yet powerful statistical model that allows for coherent text generation without deep learning techniques.

This task is part of my **ProGen AI Internship (Task-03)**, and it helped deepen my understanding of how probability-based models like Markov Chains can be used for text generation.

## Table of Contents
- [Introduction](#introduction)
- [Features](#features)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Usage](#usage)
- [Example](#example)
- [Customization](#customization)
- [Credits](#credits)

## Introduction

The project builds a Markov Chain from a given text corpus, predicting the likelihood of the next word or character based on previous states. By adjusting the `state_size`, the model can control how much context it uses when making predictions.

### Key Highlights:
- **Preprocessing**: Text is preprocessed by tokenizing, removing stopwords, and normalizing the characters.
- **Markov Chain**: A statistical model based on the transition of states (words or characters).
- **Text Generation**: The generated text is random but follows the structure of the input text, creating realistic sentences.

## Features

- Build Markov Chains from any text corpus.
- Generate text with variable length and context depth (`state_size`).
- Customizable preprocessing to fit different datasets.
- Fast and efficient, capable of generating results quickly for short sequences.

## Prerequisites

Before running the project, make sure you have the following installed:

- Python 3.x
- NLTK (Natural Language Toolkit)
- random (Python standard library)

Install the required dependencies using the following command:

```bash
pip install nltk
```

## Installation

1. Clone the repository:

```bash
git clone https://github.com/your-repo/markov-chain-text-generation.git
cd markov-chain-text-generation
```

2. Download the required NLTK resources:

```python
import nltk
nltk.download('punkt')
nltk.download('stopwords')
nltk.download('gutenberg')
```

## Usage

1. **Build the Markov Chain**:
   You can modify the `state_size` to define how many previous words should influence the next prediction.

```python
markov_chain = build_markov_chain(text, state_size=2)
```

2. **Generate Text**:
   Specify the length of the generated text.

```python
generated_text = generate_text(markov_chain, length=10, state_size=2)
```

### Example of Text Generation:

```python
Generated text: "captain ahab said nothing but the whale swims by the ship"
```

## Example

The following code builds a Markov Chain and generates text based on **Moby Dick**:

```python
import random
import re
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import gutenberg, stopwords

# Preprocess the text: lowercase, remove special characters, stopwords, and tokenize
def preprocess_text(text):
    text = text.lower()
    text = re.sub(r'[^a-z\s]', '', text)
    tokens = word_tokenize(text)
    stop_words = set(stopwords.words('english'))
    tokens = [word for word in tokens if word not in stop_words]
    return tokens

# Build the Markov chain
def build_markov_chain(text, state_size=1):
    markov_chain = {}
    text = preprocess_text(text)

    for i in range(len(text) - state_size):
        state = tuple(text[i:i + state_size])
        next_word = text[i + state_size]

        if state not in markov_chain:
            markov_chain[state] = []
        markov_chain[state].append(next_word)

    return markov_chain

# Generate text
def generate_text(markov_chain, length=10, state_size=1):
    start_state = random.choice(list(markov_chain.keys()))
    current_state = start_state
    generated_text = list(current_state)

    for _ in range(length):
        next_words = markov_chain.get(current_state)
        if not next_words:
            break
        next_word = random.choice(next_words)
        generated_text.append(next_word)
        current_state = tuple(generated_text[-state_size:])

    return ' '.join(generated_text)

# Example usage
selected_text = gutenberg.raw('melville-moby_dick.txt')
state_size = 2
markov_chain = build_markov_chain(selected_text, state_size=state_size)

# Generate and print some text
print(generate_text(markov_chain, length=10, state_size=state_size))
```

## Customization

- **Change Corpus**: You can change the input text by using other texts from the NLTK Gutenberg corpus or providing your own custom text.
- **Adjust State Size**: Modify `state_size` to control how many previous words influence the next prediction.
- **Adjust Output Length**: Modify the `length` parameter in `generate_text` to change the size of the generated text.

## Credits

This project was completed as part of the **ProGen AI Internship**.

- **Mentor**: [ProGen AI]
- **Text Data**: [Project Gutenberg] via NLTK
- **Library**: [NLTK](https://www.nltk.org/)




