# Local SchulCloud Setup

Deploying [SchulCloud](https://github.com/hpi-schul-cloud/schulcloud-server) locally has been simplified by means of a collection of scripts.

## Prerequisites (macOS only)

Before running the local setup scripts on macOS, install all required tools as follows:

```bash
bash scripts/helper/install-prerequisites-on-mac.sh
```

The script installs each tool only if it is not already present:

| Tool                        | How it is installed                    |
| --------------------------- | -------------------------------------- |
| [Homebrew](https://brew.sh) | official install script from `brew.sh` |
| git                         | `brew install git`                     |
| Node.js                     | `brew install node`                    |
| Docker Desktop              | `brew install --cask docker`           |

Each step is idempotent — running the script again when a tool is already installed prints an info line and skips the install.

## Configuring the repos directory

By default, the SchulCloud repos are cloned into `repos/`.

To use a different location per user, create a local `.env.local` file in the project root or set `SCHULCLOUD_REPOS_DIR` in your shell:

```bash
SCHULCLOUD_REPOS_DIR="$HOME/dev/schulcloud-repos"
```

A shell-level `SCHULCLOUD_REPOS_DIR` takes precedence over `.env.local`.

## Support for third party OAuth

1. Brandenburg Schulconnex is supported for development purposes in the SchulCloud local setup. Before seeding the database, your local .env file must be updated with the following variables, the values of which may be obtained from MBJS Brandenburg:

* BRANDENBURG_CLIENT_ID,
* BRANDENBURG_CLIENT_SECRET
* BRANDENBURG_AUTH_ENDPOINT
* BRANDENBURG_TOKEN_ENDPOINT
* BRANDENBURG_JWKS_ENDPOINT
* BRANDENBURG_ISSUER
* BRANDENBURG_END_SESSION_ENDPOINT
* BRANDENBURG_PROVISIONING_URL

## Steps for a local setup

1. `scripts/steps/01-sync-schulcloud-repos.sh` — syncs `schulcloud-server`, `schulcloud-client`, and `nuxt-client` into the configured repos directory (defaults to `repos/`)
2. `scripts/steps/02-install-schulcloud-node-deps.sh` — runs `npm ci` in each repo
3. `scripts/steps/03-start-mongodb.sh` — starts MongoDB via Docker (idempotent: restarts existing container or creates a new one)
4. `scripts/steps/04-start-rabbitmq.sh` — starts RabbitMQ via Docker (idempotent: restarts existing container or creates a new one)
5. `scripts/steps/05-seed-mongodb.sh` — seeds the MongoDB by running `npm run setup:db:seed` in `schulcloud-server` (skips if already seeded)
6. `scripts/steps/06-start-backend.sh` — starts the backend with `npm run nest:start:dev` in `schulcloud-server`
7. `scripts/steps/07-build-and-watch-schulcloud-client.sh` — run `npm run build` and then `npm run watch` in `schulcloud-client`
8. `scripts/steps/08-serve-nuxt-client.sh` — run `npm run serve` in `nuxt-client`

After following all steps and confirming that the docker containers are running, access the local SchulCloud via:

- http://localhost:3000/api/v3/docs - backend
- http://localhost:4000/ - Vue/prod client
- http://localhost:3100/ - legacy client (NOT relevant for local setup and used for specific proxy tests and debugging only - shows error on access)

## Helper scripts

- `scripts/helper/local-config.sh` — load `.env.local` and resolve the repos directory used by the setup scripts
- `scripts/helper/install-prerequisites-on-mac.sh` — install required macOS tools before running the setup steps
- `scripts/helper/connect-mongodb.sh` — open an interactive `mongosh` shell in the MongoDB container started by `scripts/steps/03-start-mongodb.sh`
