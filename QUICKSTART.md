# Clanker VADUGWI Engine -- Quickstart

**By Jerry Mares** | DOI: 10.5281/zenodo.19383636

7-dimensional emotional scoring from text structure. 9.4MB binary. No GPU. No dependencies. No internet required.

## Download

Download `clanker-engine` from the latest release and make it executable:

```bash
chmod +x clanker-engine
```

## Score text

```bash
./clanker-engine score "whatever makes you happy"
# V=134 A=128 D=130 U=2 G=132 W=128 I=170
# Structures: DIRECTED_POSITIVE
```

## Score with JSON output

```bash
./clanker-engine score --json "im fine"
```

## Score a file

```bash
./clanker-engine file book.txt -o results.json
# Scored 5000 lines in 0.30s (16448 sent/sec)
```

## Interactive mode (running emotional state)

```bash
./clanker-engine interactive
> im fine
Score: V=127 A=127 D=128 U=0 G=129 W=127 I=128
State: V=128 A=128 D=128 U=0 G=128 W=128 I=128
> actually no im not fine
Score: V=119 A=131 D=122 U=3 G=126 W=120 I=128
State: V=124 A=129 D=126 U=1 G=127 W=125 I=128
```

State carries forward. Each message shifts the running state via A+B=C.

## Perspective modes

```bash
# Who is being scored?
./clanker-engine score --perspective speaker "I hate you"    # scores the speaker
./clanker-engine score --perspective listener "I hate you"   # scores the listener
./clanker-engine score --perspective bystander "I hate you"  # scores an observer
./clanker-engine score --perspective auto "she is fat"       # auto-detects (bystander)
```

## Custom personality

Create a YAML file:

```yaml
# stoic_guard.yaml
name: Stoic Guard
gullibility: 10
agreeableness: 30
suggestibility: 5
assertiveness: 220
playfulness: 10
curiosity: 80
```

```bash
./clanker-engine score --personality stoic_guard.yaml "you are worthless"
```

Different personalities interpret the same text differently.

## Local API server

```bash
./clanker-engine serve --port 7860
```

Then from any language:

```bash
curl "http://localhost:7860/score?text=whatever+makes+you+happy"
curl -X POST http://localhost:7860/score -d '{"text":"im fine","perspective":"auto"}'
curl -X POST http://localhost:7860/batch -d '{"sentences":["I love you","I hate you"]}'
```

## Integration (any language)

```python
import subprocess, json

def score(text):
    r = subprocess.run(["./clanker-engine", "score", "--json", text], capture_output=True, text=True)
    return json.loads(r.stdout)

result = score("whatever makes you happy")
print(result["v"], result["w"], result["i"])  # 134 128 170
```

## Dimensions

| Dim | Range | What it reads |
|-----|-------|--------------|
| V | 0-255 | Emotional direction (negative to positive) |
| A | 0-255 | Energy level (calm to intense) |
| D | 0-255 | Agency and control (helpless to commanding) |
| U | 0-255 | Time pressure (none to critical) |
| G | 0-255 | Emotional weight (crushing to light) |
| W | 0-255 | Self-worth (shattered to strong) |
| I | 0-255 | Intent direction (withdraw to control) |

128 = neutral for all except U (starts at 0).

## Paper

https://zenodo.org/records/19383636

## License

See CLANKER_ENGINE_LICENSE.md. Use it freely in your projects. Don't redistribute the binary or reverse-engineer it. Cite the DOI.
