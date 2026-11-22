# PitchFlag – Freelancer Proposal Generator (Toggle War)

A single-file web app that helps freelancers write Upwork/Fiverr cover letters, and uses **Flagsmith** to A/B test what works.

## Problem
Freelancers (especially students and new sellers) lose jobs to generic proposals. Which tone actually gets copied and used? PitchFlag answers that with real feature-flag experiments instead of guesses.

## How Flagsmith is used
| Flag | Type | Values | Purpose |
|---|---|---|---|
| `proposal_tone` | Remote config + multivariate | `professional` / `friendly` / `confident` (about 33% each) | Decides the tone of the generated proposal |
| `landing_headline_variant` | Remote config + multivariate | `A` / `B` (50/50) | Tests two landing-page headlines |

- Each visitor gets a stable random identity (stored in `localStorage`), so Flagsmith always serves them the same variant.
- The app calls the Flagsmith Edge API (`/api/v1/identities/`) with the environment key. No SDK or build step.
- The page shows a **flag ticket** with the variants this visitor received and where they came from.
- **Metric:** per tone, the app counts proposals generated, **Copy** clicks and **👍** clicks, and shows a results table with copy rate.
- If Flagsmith is unreachable, the app falls back to `professional` / `A`.

## Run it
1. In Flagsmith, create the two flags above (Server-side only **OFF**) and copy the **Environment ID**.
2. Open `index.html` in a browser (or deploy it to GitHub Pages / Netlify).
3. Open **Settings**, paste the Environment ID, and select **Save settings**. (You can also set `ENV_ID` at the top of the script.)
4. Optional: paste an Anthropic API key to generate proposals with Claude. Without it, built-in tone templates are used.

## Demo script (2–3 minutes)
1. Show the Flagsmith dashboard with both flags and their weights.
2. Open the app: point at the flag ticket (tone + headline for this visitor).
3. Paste a job post, generate, copy, and give a 👍. Watch the results table update.
4. In Flagsmith, change the control value or weights, reload the app, and show the new variant live.

## Notes
- The Environment ID is a client-side key and safe to expose. Do not put server-side keys in this repo.
- Results are stored locally for the demo. For production, send events to an analytics backend.
