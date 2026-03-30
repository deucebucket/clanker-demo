# Clanker Demo

A live demo of the Clanker conversation state resolver. Try it at [deucebucket.github.io/clanker-demo](https://deucebucket.github.io/clanker-demo/).

## What is Clanker?

Clanker reads text the way a chess player reads a board -- recognizing structural patterns, not memorizing specific sentences. It computes 5D emotional coordinates called VADUG:

- **V** -- Valence (positive/negative)
- **A** -- Arousal (calm/activated)
- **D** -- Dominance (submissive/dominant)
- **U** -- Urgency (low/high stakes)
- **G** -- Gravity (emotional weight)

"Whatever" alone reads as resignation. "Whatever makes you happy" reads as passive-aggressive. "Do whatever" reads as permission. Same word -- context changes the score. A sentiment classifier says "neutral" for all three.

## What the Demo Shows

Two characters face each other. You type as either side and see the emotional impact on the receiver.

- Type in A's text box -- B reacts
- Type in B's text box -- A reacts
- Press Enter to send the message to the conversation log

Each message gets a VADUG score, an emotion label, and detected structural patterns. The characters visually respond -- posture, expression, and color shift based on the emotional state.

## Features

- **5 selectable characters** with distinct appearances and idle animations
- **Speech bubbles** showing what was said
- **Emotion labels** that update in real time as you type
- **VADUG bars** showing the 5D score for each character
- **Trauma tracking** -- repeated negative input accumulates visible damage
- **Persistent memory** -- characters remember their emotional state across sessions (localStorage)
- **Conversation log** with per-message VADUG scores and detected structures
- **Word role chips** showing how the engine classified each word
- **Mood graphs** tracking emotional trajectory over the conversation

## How It Works

The engine classifies every word into structural roles (emotional, negator, amplifier, connector, self-reference, etc.), computes proximity-based influence fields between words, and detects structural patterns in the role sequences. Connectors act as math operators: "and" adds, "but" subtracts, "or" branches, "if" conditionalizes.

There is no machine learning model running in the browser. The vocabulary and role data are loaded from JSON files, and the scoring runs as deterministic math.

## Numbers

- ~300KB engine (single HTML file, zero dependencies)
- 0.15ms per sentence
- 2,400+ vocabulary words with curated force vectors
- 22 structural pattern detectors
- Zero external dependencies

## Source

The main engine repository is at [github.com/deucebucket/clanker](https://github.com/deucebucket/clanker) (private).
