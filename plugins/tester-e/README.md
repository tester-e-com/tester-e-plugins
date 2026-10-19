# tester-e plugin for Claude Code

Claude writes WEB, API, performance and mobile tests for your app, keeps them in your
repository under `.tester-e/`, registers them in a tester-e project as test cases → suites →
plans, runs them, and adds a CI job. The files are the source of truth: syncing again only
changes what changed, and a test removed from the repository is marked stale, never deleted.

**You need** a tester-e account and a project you can edit. Creating and running tests spends
credits, as in the app. The CI job needs a CI/CD API key (PRO and ENTERPRISE).

## Install

```text
/plugin marketplace add tester-e-com/tester-e-plugins
/plugin install tester-e@tester-e
/reload-plugins
```

Then run `/mcp`, choose **plugin:tester-e:tester-e** and **Authenticate**. Sign in with your
tester-e account. If tester-e is also connected to your claude.ai account, Claude Code shows
that connector too — keep one of the two.

If the plugin's server does not connect, add it by hand once:

```bash
claude mcp add --transport http tester-e https://app.tester-e.com/mcp \
  --client-id tester-e-mcp --callback-port 33418
```

Always type the commands with their prefix (`/tester-e:init`) — Claude Code has its own `/init`.

- **Update:** `/plugin marketplace update tester-e`, then `/reload-plugins`.
- **Remove:** `/plugin uninstall tester-e@tester-e` — tests in tester-e and your `.tester-e/`
  files stay.
- **Rub**
  `/plugin marketplace remove tester-e` and install again with the commands above.

## Use

| Command | What it does | Credits |
|---|---|---|
| `/tester-e:init [project]` | Pick the project, write `.tester-e/config.yaml`, create the test environment | none |
| `/tester-e:pull [plan or suite]` | Bring tests that already exist in tester-e into the repo as files and attach them | none |
| *"write tests for the checkout flow"* | Claude writes `.tester-e/tests/<suite>/<name>.yaml` (WEB, API, performance, mobile) | none |
| `/tester-e:sync [--prune]` | Validate, then create/update cases, suites and plans | creating a case costs its steps; unchanged cases cost nothing |
| `/tester-e:execute [plan \| suite \| test key …] [env=…]` | Run with self-healing **off**, report each failure with its step | executed steps |
| `/tester-e:ci [github \| gitlab \| azure]` | Write a GitHub Actions, GitLab CI or Azure Pipelines job that runs the plan | CI runs: executed steps |

- A **test key** is the file path under `.tester-e/tests/` without `.yaml`, e.g.
  `cart/add-first-product-updates-badge`.
- **Performance** tests (Lighthouse or k6) need at least one threshold; a performance URL or a
  k6 run costs 10 credits.
- **Mobile** tests run only on a device farm configured under `mobile:` in `config.yaml`
  (an emulator or a local device cannot be reached from a remote run); the CI job leaves them out.
- Tests that disappear from the repo are tagged `tester-e:stale` and set to Draft (`--prune`).
  They are never deleted; run history and linked bugs stay.
- Passwords and tokens never go into `.tester-e/`. They are secret variables of the tester-e
  environment (Test Environments in the app).

Full guide: **Plugins → Claude Code** in the [tester-e docs](https://app.tester-e.com/docs).
