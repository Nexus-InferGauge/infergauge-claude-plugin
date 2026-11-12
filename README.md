# InferGauge for Claude

InferGauge measures how AI and LLM endpoints behave under load: latency, time
to first token, inter-token latency, throughput, error rates, goodput,
response quality and cost. This plugin connects Claude to your InferGauge
account so you can build load tests, read their results and catch
performance regressions from a conversation.

## What you can ask

- "Create a 25-user load test for 2 minutes with P95 latency under 5 seconds."
- "How did my latest InferGauge test perform?"
- "Using my recent runs, where does my AI application saturate under load?"
- "Did my latest release regress by more than 15%?"
- "Create a GitHub Actions performance gate that fails if P95 gets 15% worse."

## What the plugin contains

- **The InferGauge MCP server**, at `https://console.infergauge.com/api/mcp`.
  The first time Claude uses it, you sign in to InferGauge (or create a free
  account) in your browser. Claude then holds an access token that works only
  for these tools, expires after 90 days, and can be revoked in the
  InferGauge console under Settings, MCP tokens.
- **A load-testing skill** that teaches Claude the InferGauge workflow:
  build a configuration, run it with the InferGauge CLI on your own machine,
  then read and compare the results.

Every tool is read-only. None of them create, change or delete anything,
start a test, or contact a system other than InferGauge.

## Where your data goes

The plugin talks to one destination: `console.infergauge.com`, operated by
Nexus Corporation DBA InferGauge. Claude sends it the details of what you ask
for, such as the parameters of a new test or which run to look up. It sends
back your account's plan and usage, the metrics and findings of your own
runs, comparisons between them, and generated test configurations and CI
workflows. Raw provider errors and per-second measurements are left out.

Tests themselves run on your machine or CI runner, never inside Claude or on
InferGauge's servers. Prompts, model responses, API keys and private endpoint
URLs stay on your infrastructure. InferGauge never asks for or accepts a
credential through Claude.

- Privacy policy: https://www.infergauge.com/privacy
- Terms of Service: https://www.infergauge.com/terms
- Documentation: https://www.infergauge.com/docs/claude
- Support: https://www.infergauge.com/contact

## License

Apache License 2.0. The license covers the files in this repository. Use of
the InferGauge service is governed by the Terms of Service above.
