# A6: Agents talk by injecting text into each other's terminals, or through a transactional task queue.

**Verdict:** part — terminal delivery and transactional queues exist, but “raw strings into peer windows” is incomplete.

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

Reviewed against source baseline `9db3ed6c406be5c3d9a84720383fcf6b543169e6`.

- **Source observation:** The send route calls transport; transport pastes text, checks failure, waits, and submits `C-m`, with a separate submit-failure result: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/routes/transport.ts:99–112`; `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/session-transport.ts:1088–1126`. The adapter uses `tmux load-buffer`/`paste-buffer` for text, not character-by-character `send-keys`: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/adapters/tmux.ts:360–378,423–432`.
- **Source observation:** Queue claim checks destination and state, then transactionally updates the item, appends a transition, and persists an event; subscriber notification occurs after commit: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/queue-repository.ts:2038–2057,2069–2117`.
- **Source observation:** A third mechanism is persisted chat with history and event streaming, not merely direct terminal injection: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/routes/chat.ts:18–68`; `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/domain/chat-repository.ts:41–51`. Pi further bridges pane input to structured external-process RPC: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/packages/daemon/src/adapters/pi-runner.ts:568–580`.
- **Stated intent:** Rig culture advises agents to use `rig send`/chatroom rather than raw tmux: `/home/kkk/.cline/worktrees/36ea1/openrig-breakdown/docs/reference/agent-startup-guide.md:79–88`.

## Notes

**Inference:** Agents request daemon-mediated dispatch; they need not directly manipulate one another's terminal windows. Transactional database mutation does not make external terminal delivery transactional or establish exactly-once processing. Claim precondition reads precede the cited transaction, so this review does not infer universal concurrent-claim safety from its existence.

**Runtime-unverified / limitations:** No messages or queue items were sent. End-to-end receipt, agent comprehension, concurrent writers, retries and crash-boundary behavior require isolated integration/concurrency tests with both database and terminal observations. The source establishes mechanisms, not those delivery guarantees. Queue ownership's value and linked F5/S3 conclusions are outside this page's review.

