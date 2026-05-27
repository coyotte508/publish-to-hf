# publish-to-hf

A minimal demo of publishing to the [Hugging Face Hub](https://huggingface.co) from GitHub Actions **without storing an `HF_TOKEN` secret**, using [Trusted Publishers](https://huggingface.co/docs/hub/trusted-publishers).

- **GitHub repo:** [`coyotte508/publish-to-hf`](https://github.com/coyotte508/publish-to-hf)
- **Target Hub repo:** [`coyotte508/published-from-github`](https://huggingface.co/coyotte508/published-from-github)

## How it works

1. On push to `main`, the [`publish.yml`](./.github/workflows/publish.yml) workflow asks GitHub for a short-lived OIDC ID token (audience: `https://huggingface.co`).
2. It exchanges that token at `https://huggingface.co/oauth/token` for a short-lived Hub token scoped to `coyotte508/published-from-github`.
3. It uploads the contents of [`model/`](./model) to the Hub repo using `hf upload`.

No `HF_TOKEN` secret is stored anywhere in this repo or in GitHub.

## Setup checklist

To reproduce this for your own repo:

1. **Create the target Hub repo**, e.g. `your-name/published-from-github`.
2. On that repo's **Settings → Trusted Publishers**, add a publisher with:
   - **Provider:** GitHub Actions
   - **Claims:**
     - `repository` = `your-gh-name/publish-to-hf`
     - `ref` = `refs/heads/main`
     - `workflow` = `publish.yml`
3. Edit [`publish.yml`](./.github/workflows/publish.yml) and replace `coyotte508/published-from-github` with your target repo.
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
