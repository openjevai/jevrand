# OpenJEV support for jevrand

This fork adds optional [OpenJEV](https://openjev.sh) support alongside the existing TypeSafe and OpenRouter providers. OpenJEV is a free community gateway to the same Jev model built by [TypeSafe](https://typesafe.ai). TypeSafe remains the default; anyone with a `TYPESAFE_API_KEY` sees no behaviour change.

## What was added

- `jevrand/providers.py` — new `"openjev"` entry in `PROVIDERS` (endpoint `https://api.openjev.sh/v1/systemone`, model `openjev`, key env `OPENJEV_API_KEY`); HTTP 503 added to the retryable status hints; error messages updated to list all three providers.
- `jevrand/cli.py` — `--provider openjev` added to the CLI choices.
- `tests/conftest.py` — `--live-provider openjev` added to the live-test options.
- `tests/test_providers.py` — `openjev` wire-contract case, `OPENJEV_API_KEY` cleanup in the environment fixture, and updated error-message assertions.
- `tests/test_cli.py` — `OPENJEV_API_KEY` cleanup in fixtures and the fresh-process env filter.
- `.env.example` — `OPENJEV_API_KEY=` line added.
- `README.md` — OpenJEV note after the intro, provider selection text, option/field/provider tables, and docs link updated.

## Provider selection rule

1. Explicit `--provider openjev` (CLI) or `provider="openjev"` (library) wins and requires `OPENJEV_API_KEY`.
2. Otherwise, if `TYPESAFE_API_KEY` is set → TypeSafe (unchanged default).
3. Otherwise, if `OPENROUTER_API_KEY` is set → OpenRouter.
4. Otherwise, if `OPENJEV_API_KEY` is set → OpenJEV.

## Configuration

```sh
export OPENJEV_API_KEY='your-openjev-key'
jevrand --provider openjev
```

Get a free key at https://openjev.sh/dashboard. Keys come from the process environment; jevrand does not load `.env` files.

## Verification

A live `POST https://api.openjev.sh/v1/systemone` request with model `openjev`, state `ping`, and one noul question returned HTTP 200. No repository code was executed. A grep confirms no hardcoded `api.typesafe.ai` default was introduced or altered — TypeSafe's entry is untouched.

## Upstream

Original project: https://github.com/fluffypony/jevrand by @fluffypony (BSD 3-Clause).
