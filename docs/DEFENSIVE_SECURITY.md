# Defensive Sandboxing & Authorized Security Learning

This guide is for protecting Claupper and learning security in systems you own or have explicit permission to test. Claupper is a Flipper Zero HID remote and offline manual; it does **not** sandbox Claude Code, Grok, terminal commands, or the computer it is paired with. Configure protections on the machine where the agent and its tools actually run.

## Priorities

1. **Protect the host first.** Keep the OS, Flipper firmware, ufbt/SDK, compilers, and dependencies current. Use a standard user account; keep secrets out of the repository and logs.
2. **Sandbox agent processes on the execution host.** In supported VS Code Copilot harness sessions, enable `chat.agent.sandbox.enabled` on the host that runs commands, satisfy its OS prerequisites, start a new session, and inspect `/sandbox policy`. This setting is specific to that harness; do not assume Claude Code, Grok, custom tools, or another agent uses it.
3. **Use least privilege.** Give an agent access only to the project and files needed. Deny sensitive directories, minimize developer-tool and credential grants, and avoid administrator/root execution.
4. **Treat networking separately.** Sandbox filesystem controls do not imply network isolation. Disable outbound access if unnecessary; otherwise allow only the services required. Do not expose local-network services without a clear need.
5. **Keep approval gates.** Review commands that modify the host, install packages, change device firmware, access credentials, or leave the sandbox. Never accept an outside-sandbox retry just to bypass a restriction you have not understood.
6. **Test in a disposable lab.** Use a throwaway VM/container or isolated test device with synthetic data. Keep it separate from personal accounts, production systems, and networks. Take a snapshot before experiments.
7. **Verify and recover.** Review diffs, build outputs, and logs; run tests; keep backups; and document rollback steps before changing host or device configuration.

## Example: Narrow VS Code Copilot Harness Setup

The following applies only to the documented Copilot harness using its shared-runtime default tools. Confirm your version, platform prerequisites, and effective policy before relying on it:

```json
{
  "chat.agent.sandbox.enabled": "on",
  "chat.agent.sandbox.network.allowNetwork": false,
  "chat.agent.sandbox.fileSystem.allowDevToolAccess": false,
  "chat.agent.sandbox.allowUnsandboxedCommands": false
}
```

Add only the specific read/write paths or network destinations a task requires. Some builds or organization policies may manage or override settings. The current working directory may receive automatic write access, so keep secrets and unrelated projects outside it. Run `/sandbox policy` in a supported Copilot harness session to inspect effective permissions. These settings are examples—not a configuration for Claude Code, Grok, Flipper firmware, or this C application.

## Authorized Security Learning

Practice threat modeling, secure code review, dependency auditing, static analysis, and defensive testing against Claupper or a deliberately vulnerable lab you own. Before any active test, define written scope: target, permitted techniques, time window, data handling, and stop conditions. Use test devices and synthetic accounts. Do not scan, access, disrupt, persist in, evade defenses on, or extract data from third-party systems without explicit authorization. Report findings privately and responsibly, with reproducible evidence that avoids exposing secrets.

For this project, review HID behavior, transport setup/teardown, input validation, memory/resource cleanup, dependency provenance, and build/release integrity. Keep security changes reviewable and run the project's supported build and tests. Do not add code or instructions intended to compromise systems or retaliate against suspected attackers.

## Claude Code Project Guidance

When assisting in this repository, Claude Code should:

- Treat defensive security and authorized lab work as the only supported security-testing scope.
- Ask for clear authorization and bounded scope before helping plan active testing; if scope is unclear, stick to static review, threat modeling, or defensive remediation.
- Prefer least privilege, sandboxing on the actual execution host, minimal network access, and human review of consequential actions.
- Never claim this Flipper app provides a host or agent sandbox; it only sends HID input and displays local manual content.
- Avoid exploit deployment, credential theft, persistence, stealth/evasion, destructive actions, or targeting people. Offer safe defensive alternatives instead.
- Keep discussion respectful. A little generic humor is fine; don't target, shame, threaten, deceive, or harass anyone.

## Twelve Reminders for a Resilient Security Culture

1. Scope before testing.
2. Permission before access.
3. Isolate before experimenting.
4. Minimize privileges.
5. Minimize secrets.
6. Limit network paths.
7. Review before execution.
8. Log without leaking.
9. Patch verified issues.
10. Back up before changes.
11. Report responsibly.
12. Leave people and systems safer than you found them.

A light, non-targeted team motto: **“Patch first; flex never.”** Security is about reducing risk and helping people—not humiliating a presumed adversary.
