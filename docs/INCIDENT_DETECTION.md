# Incident Detection, Debugging, and Evidence

## Scope and limitation

Claupper is a Flipper Zero HID remote and offline manual. It has no visibility into the paired computer's processes, files, authentication events, or network traffic, so it cannot detect or confirm a host breach. Do not treat HID connection status, a suspicious prompt, or an agent error as proof of compromise. Host detection must be provided by the operating system, endpoint protection/EDR, and authorized security monitoring running on the computer where the activity occurs.

This guide supports defensive investigation on systems you own or are explicitly authorized to administer. It does not identify or trace a person behind an event. IP addresses, usernames, and other indicators can be shared, spoofed, or belong to innocent intermediaries; preserve evidence and let authorized incident responders handle attribution.

## Prepare before an incident

- Keep OS security updates and reputable endpoint protection enabled; configure alerts for review by the device owner or security team.
- Use a standard account for daily work, enable MFA, and keep recovery information available.
- Keep agent sessions scoped to a dedicated workspace. Do not put credentials, private keys, or unrelated sensitive files in that workspace.
- Enable only the logging and retention needed for your environment. Protect logs from unauthorized access and avoid recording secrets, full prompts, or personal data unnecessarily.
- Document who is authorized to investigate, how to contact them, and how to isolate the affected device without destroying evidence.
- Test alerting with vendor-provided test events or a controlled lab—not by running malware or probing a third-party system.

## Triage when an alert appears

1. **Validate the signal.** Record the alert source and exact event, then check the operating system or security product's official documentation. A single detection may be a false positive; do not infer compromise or attribution from a name alone.
2. **Preserve minimal evidence.** Note the UTC time, device, alert/source ID, relevant process or file identifier, and where the original logs are stored. Keep originals read-only when feasible. Redact tokens, passwords, personal data, and unrelated user content before sharing.
3. **Contain safely.** If the alert indicates active compromise, follow your organization's incident process. Disconnect the affected device from networks only when safe and authorized; avoid wiping, reinstalling, or deleting files before responders decide whether evidence is needed.
4. **Review access.** From a known-clean device, revoke or rotate credentials that may have been exposed, starting with high-impact accounts. Use the provider's official account-security workflow and preserve recovery access.
5. **Escalate and recover.** Contact your administrator or incident-response provider. Patch or restore from a known-good backup after the scope is understood. Record the remediation and verify normal operation.

## Debug without leaking data

For a focused investigation, collect only what is needed and use trusted OS/vendor tools:

- Time window (UTC) and time synchronization status.
- Alert name, provider, event/record ID, and severity as reported by the tool.
- Affected device and logged-in account identifier (redacted where appropriate).
- Relevant process/file metadata and the associated event-log location; avoid copying file contents unless responders explicitly need them.
- Recent authorized changes that could explain the event, such as a software update or development build.

Do not disable protection, run unknown cleanup scripts, upload suspicious files to public scanners, or paste logs containing secrets into an AI chat. For a suspicious file, preserve it and follow the endpoint-security vendor's quarantine and evidence-handling instructions.

## Detection summary format

When presenting a finding, separate observation from interpretation. For example:

```text
Status: Needs review (not confirmed compromise)
Observed: [alert/source and exact event ID]
Time: [UTC timestamp and investigation window]
Asset: [device identifier, appropriately redacted]
Evidence: [protected log location or case reference; no secrets]
Confidence: [low / medium / high, with reason]
Benign explanations checked: [updates, approved tools, expected activity]
Immediate action: [containment or escalation, if authorized]
Next step: [owner and time]
```

Only say “confirmed breach” when an authorized responder has validated sufficient evidence. If the event is not confirmed, state the uncertainty and the next safe verification step. Never claim to have traced or identified an attacker based only on an alert.

## Agent and Claupper boundaries

An AI coding agent can help review sanitized logs or source code, explain security-tool findings, and suggest defensive fixes. It should not be given credentials or asked to access another party's systems. Require explicit authorization and bounded scope for active testing; otherwise limit help to static analysis, hardening, and incident-response documentation. Claupper itself does not monitor the paired computer or produce breach alerts.
