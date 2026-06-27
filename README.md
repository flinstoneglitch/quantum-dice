# 🎲 q20 — Quantum Dice

A browser dice roller (d4 · d6 · d8 · d10 · d12 · d20 · d100) whose daily entropy seed is sourced from the QeNTROPY Engine.

**Play:** https://flinstoneglitch.github.io/quantum-dice/

## How it works

On page load, the game fetches a stable daily entropy seed from the QeNTROPY public API:

```
GET https://q-runtime.onrender.com/api/daily-seed?app=q20
```

The seed is the same for all players on the same LA calendar day and never changes mid-session. Each individual roll is derived deterministically from:

```
fnv32a(dailySeed : sides : rollCounter : sessionNonce)
```

- **dailySeed** — fixed for the day; same for everyone
- **sides** — die type (4, 6, 8, 10, 12, 20, or 100)
- **rollCounter** — increments each roll so results vary within a session
- **sessionNonce** — random per page load so different sessions get different sequences

No `Math.random()` is used when the QeNTROPY seed is available. No private API keys are embedded in the frontend.

## Fallback chain

1. QeNTROPY daily seed (`/api/daily-seed?app=q20`) — public endpoint, no key required
2. Local `seed.json` (static fallback, deterministic)
3. Deterministic LA-date hash (always reproducible, never random)

## Stack

- `index.html` — the entire game (vanilla JS, no frameworks)
- `seed.json` — static fallback seed
- `.github/workflows/update-seed.yml` — scheduled seed refresh
- Hosted on GitHub Pages, auto-deployed from this repo

## Credits

Created by Colin O'Reilly (Colin O'Reilly Studios / Artphorm),
built with AI collaborators: Claude (Anthropic) and ChatGPT (OpenAI).

## License

MIT
