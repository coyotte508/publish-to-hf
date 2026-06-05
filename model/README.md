---
license: mit
tags:
  - demo
  - trusted-publishers
  - github-actions
---

# published-from-github

This repo is published automatically from [`coyotte508/publish-to-hf`](https://github.com/coyotte508/publish-to-hf) on GitHub, using Hugging Face [Trusted Publishers](https://huggingface.co/docs/hub/trusted-publishers) — no `HF_TOKEN` secret stored anywhere.

Every push to `main` on the GitHub repo:

1. Mints a short-lived GitHub OIDC ID token (audience: `https://huggingface.co`).
2. Exchanges it at `https://huggingface.co/oauth/token` for a short-lived Hub token scoped to this repo.
3. Uploads the contents of [`model/`](https://github.com/coyotte508/publish-to-hf/tree/main/model) here.

See the [Trusted Publishers documentation](https://huggingface.co/docs/hub/trusted-publishers) for the full walkthrough.

<!-- bump 3 -->
