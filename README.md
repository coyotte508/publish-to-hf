# publish-to-hf

A minimal demo of publishing to the [Hugging Face Hub](https://huggingface.co) from GitHub Actions **without storing an `HF_TOKEN` secret**, using [Trusted Publishers](https://huggingface.co/docs/hub/trusted-publishers).

- **GitHub repo:** [`coyotte508/publish-to-hf`](https://github.com/coyotte508/publish-to-hf)
- **Target Hub repo:** [`coyotte508/published-from-github`](https://huggingface.co/coyotte508/published-from-github)

## How it works

1. On push to `main`, the [`publish.yml`](./.github/workflows/publish.yml) workflow runs `hf upload` with the `HF_OIDC_RESOURCE` environment variable set to the target repo.
2. The `hf` CLI detects it's running on GitHub Actions, requests a short-lived OIDC ID token from GitHub, and exchanges it at `https://huggingface.co/oauth/token` for a short-lived Hub token scoped to `coyotte508/published-from-github` — all automatically.
3. The contents of [`model/`](./model) are uploaded to the Hub repo.

No `HF_TOKEN` secret is stored anywhere in this repo or in GitHub.

## Setup checklist

To reproduce this for your own repo:

1. **Create the target Hub repo**, e.g. `your-name/published-from-github`.
2. On that repo's **Settings → Trusted Publishers**, add a publisher with:
   - **Provider:** GitHub Actions
   - **Claims:**
     - `repository` = `your-gh-name/publish-to-hf`
     - `branch` = `main`
     - `workflow` = `publish.yml`
3. Edit [`publish.yml`](./.github/workflows/publish.yml) and replace `coyotte508/published-from-github` with your target repo (in both `HF_OIDC_RESOURCE` and the `hf upload` command).
4. Push to `main` (or trigger the workflow manually) — done.

## Trigger a publish

- Push any change to `main`, or
- Run the workflow from GitHub's **Actions** tab via **Run workflow** (`workflow_dispatch`).

## Layout

```
.
├── .github/workflows/publish.yml   # The CI workflow
├── model/                          # What gets uploaded to the Hub
│   └── README.md                   # Becomes the model card
└── README.md                       # You are here
```
