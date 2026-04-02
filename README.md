# Clanker VADUGWI Engine

**By Jerry Mares** | [Paper](https://zenodo.org/records/19383636) | [Live Demo](https://huggingface.co/spaces/deucebucket/clanker) | [DOI: 10.5281/zenodo.19383636](https://zenodo.org/records/19383636)

A deterministic engine that computes 7-dimensional emotional coordinates from text structure. 9.4MB compiled binary. 0.1ms per sentence. No GPU. No dependencies. No internet required.

"Whatever" alone reads as resignation. "Whatever makes you happy" reads as passive-aggressive. "Do whatever" reads as permission. Same word, different structure, different score. A sentiment classifier says "neutral" for all three.

## Download

Grab the compiled binary from the [latest release](https://github.com/deucebucket/clanker-demo/releases/latest).

```bash
chmod +x clanker-engine
./clanker-engine score "whatever makes you happy"
# V=134 A=128 D=130 U=2 G=132 W=128 I=170
# Structures: DIRECTED_POSITIVE
```

See [QUICKSTART.md](QUICKSTART.md) for full usage.

## What it scores

7 dimensions, each 0-255. 128 = neutral (except Urgency which starts at 0).

| Dim | What it reads |
|-----|--------------|
| **V** Valence | Emotional direction (negative to positive) |
| **A** Arousal | Energy level (calm to intense) |
| **D** Dominance | Agency and control (helpless to commanding) |
| **U** Urgency | Time pressure (none to critical) |
| **G** Gravity | Emotional weight (crushing to light) |
| **W** Self-Worth | Self-assessment (shattered to strong) |
| **I** Intent | Communicative direction (withdraw to control) |

7 bytes encode 72 quadrillion unique emotional states.

## How it works

Four layers run in sequence:

1. **Word classification** -- each word gets a structural role (SELF_REF, EMOTIONAL, NEGATOR, AMPLIFIER, CONNECTOR, etc.)
2. **Proximity fields** -- nearby words influence each other with exponential decay (0.7x per word)
3. **Structure detection** -- 26 named patterns detected from role arrangements (sarcasm, betrayal, crisis, passive-aggression, etc.)
4. **Physics** -- momentum-based force blending produces final 7D coordinates

Plus: force flow resolution (WHO does WHAT to WHOM), perspective modes (speaker/listener/bystander), personality vectors, and A+B=C state transitions.

## Modes

```bash
# Score a sentence
./clanker-engine score "im fine"

# Score with perspective
./clanker-engine score --perspective bystander "she is fat"
./clanker-engine score --perspective auto "you are worthless"

# Score a file (16,000+ sentences/sec)
./clanker-engine file book.txt -o results.json

# Interactive with running state
./clanker-engine interactive

# Local API server
./clanker-engine serve --port 7860
```

## Integration

From any language:

```python
import subprocess, json

def score(text):
    r = subprocess.run(["./clanker-engine", "score", "--json", text], capture_output=True, text=True)
    return json.loads(r.stdout)

scores = score("whatever makes you happy")
# {"v": 134, "a": 128, "d": 130, "u": 2, "g": 132, "w": 128, "i": 170, "structures": ["DIRECTED_POSITIVE"], ...}
```

Or use the local API server:

```bash
curl "http://localhost:7860/score?text=im+fine"
curl -X POST http://localhost:7860/batch -d '{"sentences":["I love you","I hate you"]}'
```

Or use the free hosted API (no download needed):

```python
from gradio_client import Client
client = Client("deucebucket/clanker")
result = client.predict("im fine", api_name="/score_text")
```

## Custom personalities

Create a YAML file:

```yaml
name: Insecure NPC
gullibility: 150
agreeableness: 200
suggestibility: 180
assertiveness: 40
playfulness: 20
curiosity: 80
```

```bash
./clanker-engine score --personality insecure.yaml --perspective bystander "she is fat"
# Low-W bystander takes the hit (self-projection)
```

## Numbers

| Metric | Value |
|--------|-------|
| Binary size | 9.4 MB |
| Vocabulary | 4,098 words with 7D force vectors |
| Structural patterns | 26 |
| Throughput | 16,000+ sentences/sec |
| Latency | 0.1ms per sentence |
| GPU required | No |
| Dependencies | None |
| Test suite | 167 tests passing |

Scored 63,128 sentences from 15 novels, 117,000 Twitch chat messages, 10,013 philosophical texts, and 521 game dialogue lines. Validated against 4 frontier language models (GPT-4, Claude, Gemini, Grok) with 76.3% consensus agreement.

## Demos

- [Live demo](https://huggingface.co/spaces/deucebucket/clanker) -- score text, watch characters argue, upload files, compare base vs engine-mounted LLM output
- [Emotional assessment](https://deucebucket.github.io/clanker-demo/assessment.html) -- 35 situational questions that map your emotional baseline across all 7 VADUGWI dimensions

## Paper

Mares, J. (2026). *VADUGWI: Deterministic 7-Dimensional Emotion Coordinates via Structural Pattern Recognition.* DOI: [10.5281/zenodo.19383636](https://zenodo.org/records/19383636)

## License

The compiled binary is free to use in your projects. See [CLANKER_ENGINE_LICENSE.md](CLANKER_ENGINE_LICENSE.md).

Do not redistribute the binary or attempt to reverse-engineer it. Cite the DOI in any published work.

Source code is proprietary. The [paper](https://zenodo.org/records/19383636) describes the full architecture for anyone who wants to build their own implementation.
