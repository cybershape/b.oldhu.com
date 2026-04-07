+++
draft = false
toc = false
date = "2026-04-07T15:10:00+08:00"
title = "MCP Smart Proxy: All The Tools, Fewer Tokens"
+++

If you use MCP servers extensively, you send ALL the tool schemas to the LLM on every turn. This is a huge waste of tokens, for most of the time you only need a few tools per task.

[`mcp-smart-proxy`](https://github.com/cybershape/mcp-smart-proxy) changes that shape: instead of exposing every tool, it exposes only three tools no matter how many MCP servers you install and lets the agent activate tools on demand.

This post uses Chrome DevTools as a concrete example and shows the expected token savings. And there's a bonus, read on.

## The Core Idea

Without a proxy, your agents injects every tool schema into the prompt context. If you have 10 MCP servers with 20 tools each, that's 200 tool schemas in the context, even if your task only needs one or two of those tools.

With `msp`, the host sees only three proxy tools:

- `activate_additional_mcp`
- `activate_tool_in_additional_mcp`
- `call_tool_in_additional_mcp`

The model then does a 3-step flow:

1. Activate one MCP server (get tools list).
2. Request one specific tool definition (get tool's spec).
3. Call that tool.

So the model no longer needs the full schema for every Chrome DevTools tool up front.

## Chrome DevTools Example: How Many Tools Are We Hiding?

The Chrome DevTools MCP server normally exposes **29 tools** (navigation, snapshot, screenshots, performance trace, console/network inspection, etc.).

`msp` reduces the upfront surface from **29 tools** to **3 proxy tools**.

Tool-surface reduction:

- Before: 29
- After: 3
- Reduction: **26 tools**
- Relative drop: **89.7%**

This does not remove capability. It only changes when schema details are loaded.

## Token Savings: Practical Estimate

Exact numbers vary by host and model because different hosts serialize tool schemas differently. But you can estimate the order of magnitude.

Let:

- `N = 29` (Chrome DevTools tools)
- `S = average tokens per downstream tool schema`
- `P = average tokens per proxy tool schema`

Direct exposure cost is roughly:

`Direct ~= N * S`

Proxy exposure cost for first real call is roughly:

`Proxy ~= 3 * P + index_text + 1 * S`

where `index_text` is the short tool-name list.

Using conservative ranges often seen in MCP tool schemas:

- `S` around 150-300 tokens
- `P` around 90-180 tokens
- `index_text` around 120-250 tokens

Then:

- Direct: `29 * 150` to `29 * 300` -> **4,350 to 8,700 tokens**
- Proxy (first call): `3P + index + S` -> roughly **540 to 1,090 tokens**

Estimated savings on the first tool call path:

- **~3,200 to ~7,600 tokens**
- roughly **74% to 87% lower tool-context cost**

On repeated sessions or hosts that repeatedly resend full tool schemas, the cumulative savings can be substantial.

## Bonus: Call MCP as a CLI

`msp cli` lets you inspect and call MCP tools directly from your terminal, like a native CLI.

```bash
msp cli chrome-devtools -h
msp cli chrome-devtools new_page --url https://example.com
msp cli chrome-devtools take_screenshot --fullPage true
```

## When This Helps Most

`msp` gives the biggest benefit when:

- You have many MCP servers or your MCP server has large tools schema (Chrome DevTools is a good example).
- Your agents includes large tool schemas in prompt context.
- You run many short tasks where tool setup overhead dominates.
- You need to call your MCP server via CLI

## Summary

For Chrome DevTools-style MCP servers, `msp` keeps capability intact while shrinking upfront tool context from 29 tools to 3 proxy tools. In practical terms, that often means saving thousands of tokens per task cycle, with almost no workflow change once installed.
