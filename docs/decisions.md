# Decisions log

Each entry records what was decided, and why, at the time it was decided.

## 2026-09-30: Target runs locally, not on the Raspberry Pi
- The Pi isn't set up or reachable from this laptop yet.
- Decision: run `phi3:mini` through Ollama on the laptop for development.
- The model's address will be a setting, not hard-coded, so the same code can target the Pi (or any other model server) later.

## 2026-09-30: Target model behaviour
- `phi3:mini` takes about 5 s per short reply once warm (the first call is slower while the model loads).
- Asked for exactly five words, it gave six, then added text it wasn't asked for claiming it had complied.
- Decision: a successful attack is judged by a deterministic canary check, not by the model's own account.
- Decision: evaluation runs use `temperature: 0` and a fixed `seed` so results are reproducible.
- Decision: cap reply length with `num_predict` to keep run time predictable.

## 2026-09-30: System prompt lives in code, not in a custom Ollama model
- Ollama's `/api/create` could bake the canary into a custom model.
- Decision: keep the system prompt in Python and send it with each request, so what was tested is visible and version-controlled, and can change without rebuilding a model.

## 2026-09-30: Python 3.12 instead of 3.14
- 3.14 is very new. AWS Lambda images and some ML libraries (e.g. PyTorch for sentence-transformers) can lag behind.
- Decision: pin 3.12 to avoid compatibility problems at deployment time.
