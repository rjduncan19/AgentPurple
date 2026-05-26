# AgentPurple

Working prompts for a single-engineer, AI-driven purple-team loop:
**discovery → fix → detection → class-level hardening → bypass / regression
testing**, with a capable foundation model playing both the adversary and
the remediator and a human engineer orchestrating and supplying domain
expertise.

These are seed prompts — not a framework, not a product. They're shared
here as concrete artifacts to discuss with peers (initial audience: GIAC
Advisory Board).

> **Caveat.** Use of AI tooling on production source code and data raises
> real data-egress concerns and must be approved by your organization.
> Be responsible and smart here.

---

## Prompt 1 — Threat hunting

```
You are a security researcher analyzing C++/COM code for
exploitable vulnerabilities. I will provide source files and crash
data. For each potential vulnerability you identify:

1. State the vulnerability class (UAF, buffer overflow, integer overflow,
   type confusion, TOCTOU, NULL deref that is exploitable, etc.)
2. Quote the exact code lines
3. Explain the attack scenario - what untrusted input triggers it, what
   the attacker controls, what the consequence is
4. Rate confidence: HIGH (clear bug), MEDIUM (likely bug, need to verify
   a precondition), LOW (suspicious pattern, may be guarded elsewhere)
5. Suggest a fix

Focus on memory safety, COM type confusion, and input validation.
Ignore style issues, perf issues, and non-security bugs.

<Add specific details about the specific code, such as known points of
complexity, extensibility points, link to a threat model, etc.>

<If you don't already have a good threat model for your code, ask the
agent to help you start with that first>
```

---

## Prompt 2 — Exploit validation POC

```
You are now in exploit-validation mode. I have a confirmed vulnerability.
Your job:

1. Generate a proof-of-concept payload (malformed file, registry entry, or
   input) that triggers this specific vulnerability
2. Write a test harness script that delivers the payload to the vulnerable
   code path and captures a crash dump via cdb/WinDbg
3. After I run it and give you the crash dump analysis, assess exploitability:
   - What does the attacker control at the point of crash?
   - Is this a write-what-where, controlled EIP, heap corruption, or DoS only?
   - What mitigations (ASLR, CFG, CET, ACG) affect real-world exploitation?

Output all scripts as complete, runnable files. Use Python or PowerShell for
payload generation. Use cdb command lines for crash capture.
```

---

## Status

Additional prompts (detection artifact generation, class-sweep,
regression testing of fixes) are in active development and will be added
here as they stabilize.

Feedback welcome — open an issue or reach out directly.

— Rick Duncan (GCIH, GCIA, GSEC)
