# A6: Agents talk by injecting text into each other's terminals, or through a transactional task queue.

**Verdict:** ? (unverified)

## What the report says

Verbatim from `sources/Cline-Vs-OpenRig-Comparison.md`. Trailing numbers are Gemini's citation markers.

> Inter-agent coordination in OpenRig occurs via direct terminal injection and centralized state tracking7.

> Agents transmit messages into peer terminal sessions through specialized dispatch commands or route structured units of work via a transactional task queue7.

> Multi-Agent Coordination Model: Addressable seats, peer terminal injection, and transactional work queues7

## What the infographic says

Verbatim from `sources/agent_fleet_architectures_infographic.html`.

> │ ↓ Injects keystrokes / commands into terminal

> Inter-agent communication works by injecting raw strings into peer terminal windows.

> Addressable Seats & Queues

## Sources Gemini cited

- [7] [GitHub - mvschwarz/openrig: Multi-agent harness that runs Claude](https://github.com/mvschwarz/openrig)

## How to check

Find the send/message command and the queue implementation. Is delivery done with `tmux send-keys`? How many separate messaging mechanisms exist (send, queue, chatrooms)?

## Evidence from this repo

_Not checked yet. Add file:line references here._

## Notes

