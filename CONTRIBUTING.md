# Contributing to Mirage

Thanks for helping. Mirage is a small project; these rules keep it easy to review.

## Before you start
- Open an issue first for anything larger than a typo or a one-line fix, so we can agree on the approach.
- Security problems: do not open a public issue. Contact the maintainers privately (GitHub: @MeghVyas3132) and give us time to fix it first.

## Development
- Requirements: Node 22+ and Docker (see the README, section 3).
- Run everything: `docker compose up -d --build`
- Tests: `npm test` from the repository root runs every workspace's tests.
- `packages/core` (`mirage-core`) must stay dependency-free. Scoring and planning changes need a test in `packages/core`.

## Pull requests
- One logical change per pull request, with a clear title and a description of what changed and why.
- Keep commits focused; write commit messages in the imperative ("Add X", "Fix Y").
- New behaviour needs a test. Bug fixes need a test that fails before the fix.
- Don't commit secrets, wallet seeds, `.env` files or real (mainnet) keys. Testnet only.
- Update the README or `docs/` when user-visible behaviour changes.

## Code style
- TypeScript, strict mode. Match the style of the surrounding code.
- Prefer small, pure functions in `mirage-core`; keep network and wallet calls in `server/`.

## Licence
By contributing, you agree that your contributions are licensed under the MIT License (see `LICENSE`).

## Code of conduct
Be respectful and constructive. Follow the Zcash community's code of conduct in discussions about this project.
