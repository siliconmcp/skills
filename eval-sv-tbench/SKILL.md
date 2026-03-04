---
name: eval-sv-tbench
description: Generate, save, and submit SystemVerilog testbenches to EaaS evaluations using the eaas MCP tools. Use when a user asks to run or submit an EaaS testbench evaluation for a specific eval set name, create a session, load testcases, submit results, or fetch session results. Requires an eval set name as input and explicitly avoids calling list-evals.
---

# Eval SV Tbench

## Overview

Create self-checking `tb.sv` files for each testcase in an EaaS eval set, submit them via the eaas MCP server, and retrieve results.

## Workflow

1. Confirm inputs.
Ask for `eval_name` if not provided. Do not call `list_evals`. Ask for optional `session_name`, `output_dir`, and `notify_email`. Use `session_name = output_dir` or `eval_name` if not specified.

2. List testcases.
Call `mcp__eaas-siliconmcp__list_eval_testcases(eval_name)` and capture the testcase names.

3. Load testcase data and stage files.
For each testcase, call `mcp__eaas-siliconmcp__get_eval_testcase(eval_name, testcase_name)`. Save files under `output_dir/<testcase_name>/`. If a `tb.sv` already exists and the user did not ask to regenerate it, keep it. Otherwise generate a new `tb.sv`.

4. Generate `tb.sv` (if needed).
Read `RefModule.sv` and `RefModule.md` to derive interface and behavior. Follow the constraints in `SAMPLE_TBENCH_GEN_PROMPT.md` if present. Make the testbench self-checking, include a hard timeout, and avoid hierarchical references or `force`/`release`. Prefer no success `$display` output; print only on failures or timeout to avoid the harness treating logs as errors.

5. Create a session.
Call `mcp__eaas-siliconmcp__create_eval_session(name=session_name)` once per run.

6. Submit results.
For each testcase, read `output_files.list` and submit only those files. Usually this is just `tb.sv`. Call `mcp__eaas-siliconmcp__submit_result(session_name, eval_name, testcase_name, result_files)`.

7. Request notification and fetch results.
After submitting results, call `mcp__eaas-siliconmcp__get_session_result(session_name, notify=notify_email)` once to register the notification (always ask for `notify_email`). Poll with `get_session_result` until `results_available` equals the number of jobs, or fetch individual results with `get_result`.

8. Handle noisy logs.
If results show simulation errors like "Test completed ..." with zero failures, remove success prints from `tb.sv` and resubmit.
