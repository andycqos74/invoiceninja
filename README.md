# Invoice Ninja — GitHub-built images for Portainer

GitHub Actions builds the images; the server only pulls them.

| Image | Built from |
|---|---|
| `ghcr.io/<owner>/invoiceninja-app` | Official `invoiceninja/dockerfiles` (debian branch) + the Invoice Ninja release tarball |
| `ghcr.io/<owner>/invoiceninja-nginx` | `nginx:alpine` + `nginx/*.conf` from this repo |

Both are tagged `latest` and with the Invoice Ninja version (e.g. `5.12.30`).

## When builds run

- **Push to `main`** or **Actions → Build Invoice Ninja images → Run workflow**: always builds (optionally for a specific version).
- **Nightly (03:17 UTC)**: builds only if a new Invoice Ninja release has appeared.

## Setup

1. Push this folder to a new GitHub repo (private is fine), branch `main`.
2. Wait for the first Actions run to finish (~10–15 min; later runs are cached).
3. Let the server pull the images — either:
   - **Public packages:** GitHub → your profile → Packages → each `invoiceninja-*` package → Package settings → Change visibility → Public, **or**
   - **Private packages:** create a classic PAT with `read:packages`, then in Portainer → Registries → Add registry → Custom: URL `ghcr.io`, username = your GitHub username, password = the PAT.
4. In Portainer → Stacks → Add stack → **Repository**: repo URL, reference `refs/heads/main`, compose path `docker-compose.yml` (add GitHub credentials if the repo is private). Or paste `docker-compose.yml` into the Web editor.
5. Environment variables → Advanced mode → paste `stack.env.example`, fill every `CHANGE_ME` (including `GHCR_OWNER`, lowercase).
6. Deploy. After first login, remove `IN_USER_EMAIL` / `IN_PASSWORD` and update the stack.

## Updating

With `TAG=latest`: in Portainer, open the stack → **Update the stack** with **Re-pull image** on.
With a pinned `TAG`: change it to the new version and update.

**Optional auto-redeploy:** enable the stack's webhook in Portainer, then add it as a repo secret named `PORTAINER_WEBHOOK` (Settings → Secrets and variables → Actions). The workflow calls it after each successful build.

Back up the `mysql_data` and `app_storage` volumes before updating.
