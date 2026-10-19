# tester-e plugins

The plugin marketplace of [tester-e](https://tester-e.com), the AI test automation platform.
A plugin brings tester-e into your AI coding assistant: the assistant writes tests next to
your code, registers them in a tester-e project and runs them there.

| Plugin | For | What it does |
|---|---|---|
| [`tester-e`](plugins/tester-e/README.md) | Claude Code | Writes WEB, API, performance and mobile tests for your app, keeps them in your repository under `.tester-e/`, catalogues them in tester-e as test cases, suites and plans, runs them and adds a CI job |

Other assistants (OpenAI Codex, Cursor, GitHub Copilot, Google Antigravity) have no plugin;
they connect to tester-e through its MCP server — see **MCP Connector** in the
[tester-e docs](https://app.tester-e.com/docs).

## Install (Claude Code)

```text
/plugin marketplace add tester-e-com/tester-e-plugins
/plugin install tester-e@tester-e
/reload-plugins
```

Then run `/mcp`, choose **plugin:tester-e:tester-e** → **Authenticate**, and sign in with your
tester-e account. The commands appear as `/tester-e:init`, `/tester-e:pull`, `/tester-e:sync`,
`/tester-e:execute` and `/tester-e:ci`. Full guide: [plugins/tester-e](plugins/tester-e/README.md)
and **Plugins → Claude Code** in the [tester-e docs](https://app.tester-e.com/docs).

**Update:** `/plugin marketplace update tester-e`, then `/reload-plugins`.

```text
/plugin marketplace remove tester-e
/plugin marketplace add tester-e-com/tester-e-plugins
/plugin install tester-e@tester-e
```

Your `.tester-e/` files and the tests already in tester-e are not affected.

## Repository layout

```text
.claude-plugin/marketplace.json   the marketplace (name: tester-e)
plugins/tester-e/                 the Claude Code plugin
  .claude-plugin/plugin.json      name, version, license
  .mcp.json                       the tester-e MCP server and its OAuth client
  skills/                         init · pull · sync · execute · ci · tester-e-tests
```

## License

MIT © tester-e.com — see [LICENSE](LICENSE).
