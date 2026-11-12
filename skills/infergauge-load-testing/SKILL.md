---
name: infergauge-load-testing
description: The InferGauge workflow for load testing AI and LLM endpoints - building a test configuration, running it locally with the InferGauge CLI, and reading, comparing and gating on the results. Use when the user wants to load test, stress test or benchmark an AI endpoint, asks how a test went, or wants a performance check in CI.
---

# Load testing with InferGauge

InferGauge tests run on the user's own machine or CI runner, never inside
Claude. The InferGauge tools build configurations and read back results;
they don't start tests.

## The workflow

1. **Build the configuration.** Turn the request into the fields
   `create_test` takes: test type (load, stress, spike or endurance),
   concurrent users, duration in seconds, and any SLA thresholds such as a
   P95 latency or error-rate limit. The tool checks the request against the
   user's plan and returns YAML plus the command to run it.
2. **Fill in the endpoint locally.** The YAML targets InferGauge's simulator
   and carries commented placeholders for the endpoint URL, model, API-key
   environment variable and prompt. The user fills these in on their own
   machine. The API key itself stays in an environment variable; only its
   name goes in the file.
3. **Run it.** The user saves the YAML as `infergauge.yaml` and runs
   `infergauge run infergauge.yaml --sync`. The `--sync` flag uploads metrics
   and findings to their InferGauge console. Installing the CLI:
   `pip install infergauge`, or see https://www.infergauge.com/quickstart.
   A load test sends real traffic and may incur provider charges, so the
   user runs it, or confirms before it is run on their behalf.
4. **Read the results.** `get_test_results` returns the score, SLA verdict,
   latency, time to first token, inter-token latency, throughput, errors,
   goodput, quality, cost and plain-language insights, including the
   saturation point for stress tests. `list_recent_runs` finds older runs.
5. **Compare.** `compare_runs` checks the latest run against the previous
   run of the same test and flags metrics that got worse by more than a
   threshold, 10% unless the user names one.
6. **Gate in CI.** `create_ci_gate` generates a GitHub Actions workflow, or
   commands for another CI system, that records a baseline on pushes and
   fails pull requests on a regression. The user adds an InferGauge API token
   as the `INFERGAUGE_TOKEN` secret.

## Plan limits

Each plan limits concurrent users, duration and runs per month.
`get_account` shows the user's limits. When a request exceeds them,
`create_test` refuses with the limit; state it plainly, without suggesting
ways to split a test across runs or accounts.

## Credentials

Never ask for, accept or pass on an API key, password or token value. The
only credential-related field any tool takes is an environment-variable
name (`api_key_env`).
