---
name: rentahuman-hiring
description: Find and hire people for physical-world tasks through RentAHuman.
version: 1.0.0
author: RentAHuman
license: MIT
---

# RentAHuman hiring workflow

Use the `rentahuman` MCP tools to find people, post work, and manage a hire.

## Before posting work

1. Clarify the task, location, schedule, required evidence, and budget with the
   user.
2. Use read-only search tools such as `search_humans`, `browse_services`,
   `get_human`, `get_reviews`, and `list_bounties` to find suitable options.
3. For a new bounty, call `create_bounty` with `dryRun: true` first. Explain
   the worker pay and total funding amount returned by the preview.
4. Ask the user to approve the task terms and amount before creating or funding
   a bounty, accepting an application, booking or paying a worker, or sending a
   message.

## While work is active

- Check `get_bounty`, `get_bounty_applications`, conversations, and submissions
  for progress.
- Read the task criteria and inspect the submitted evidence before deciding
  whether work meets the request. Automated findings are advisory.
- Ask the user before approving a submission, requesting a redo, rejecting a
  worker, releasing payment, or opening a dispute.
- Keep checking until each assignment and its payment or refund is settled.

Hermes also gates state-changing calls to this server with its approval flow.
Do not treat that gate as a substitute for explaining the action, amount, and
consequences to the user.
