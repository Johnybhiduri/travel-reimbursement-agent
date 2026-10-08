# Travel Reimbursement Approval Agent

My submission for the AI developer assignment. It is a small agent that reads a travel expense claim, checks it against the company policy, and gives a decision: APPROVE, PARTIAL_APPROVE, REJECT or MANUAL_REVIEW.

Everything is in one notebook: `jainendrabhiduri.ipynb`

## What it does

1. Takes the 5 sample claims (CLM-001 to CLM-005).
2. A LangChain agent (DeepSeek model) calls tools to check eligibility, per-diem limits, receipts and approval rules.
3. The agent gives its decision and an explanation.
4. My code validates it with Pydantic and cross checks with a plain python rule engine. If the LLM fails or disagrees, the rule engine result is used.
5. The last cell prints the JSON array. There is also a small dashboard and a dropdown UI.

## Setup

1. Make a virtual env (optional but good)
```
   python -m venv .venv
   source .venv/bin/activate
```
2. Install packages
```
   pip install -r requirements.txt
```
3. Get an API key from https://platform.deepseek.com/api_keys (you need a small balance on the account, a few dollars is more than enough)
4. Copy the env file and paste your key
```
   cp .env.example .env
```

## Environment variables

| name | needed | what for |
|---|---|---|
| `DEEPSEEK_API_KEY` | yes for LLM | your DeepSeek API key |
| `DEEPSEEK_MODEL` | no | model name, default `deepseek-flash` |

DeepSeek has changed model names a few times in 2026, so if you get a "model not found" error check the current name in their docs and set `DEEPSEEK_MODEL`.

If no key is set the notebook still runs, it just uses the rule engine only (no LLM). Good for a quick test without spending anything.

## How to run

```
jupyter notebook jainendrabhiduri.ipynb
```
Then Restart and Run All. It runs top to bottom with no manual steps. The last cell prints the JSON results.

## Key design choices

- Policy math is plain python tools, not the LLM. Rules should be correct every time.
- The LLM picks which tools to call, reads the results and writes the explanation.
- Tools only take `claim_id` (not the full claim) so the model does not have to copy big json.
- Validation + fallback so the demo does not break if the API fails or the model gives a weird answer.
- No vector db, no extra frameworks. Kept it simple on purpose.

More details are in the "Design Notes & Reasoning" section inside the notebook.

## Files

- `jainendrabhiduri.ipynb` the main notebook
- `requirements.txt` packages
- `.env.example` sample env file
- `.gitignore` keeps `.env` out of git