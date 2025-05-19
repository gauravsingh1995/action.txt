
# action.txt — Minimal, Deterministic Instruction Language for the Agentic Web

[![Spec Version](https://img.shields.io/badge/spec-v0.2-blue)](#roadmap)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)

> **TL;DR** – `action.txt` lets humans *and* AI agents automate any browser workflow with just a handful of readable lines, instead of hundreds of lines of Selenium / Playwright code, while guaranteeing deterministic, auditable runs.

---

## Table of Contents

|  Section                              |  Why you care                                              |
| ------------------------------------- | ---------------------------------------------------------- |
| [Motivation](#motivation)             | Why the agentic web needs an open DSL                      |
| [Quick Example](#quick-example)       | 6 lines that log you in and land on the dashboard          |
| [Full White Paper](#full-white-paper) | Deep‑dive spec, design philosophy, grammar, security model |
| [Roadmap](#roadmap)                   | Where the spec & tooling go next                           |
| [Repo Layout](#repo-layout)           | How this repo is organised                                 |
| [Contributing](#contributing)         | How to propose changes / new keywords                      |
| [License](#license)                   | MIT                                                        |

---

## Motivation

The **agentic web** is the next frontier where autonomous LLM‑powered agents perform end‑to‑end tasks—think *"book me the cheapest refundable Berlin→NY flight"*—by driving a browser just like you would.  Existing automation stacks are:

* **Verbose & brittle** – 200+ LOC of Selenium for a login form.
* **Opaque to non‑engineers** – Marketers can’t audit a headless‑browser script.
* **Non‑deterministic** – If a CSS selector changes, the whole flow implodes.

`action.txt` fixes that with four design pillars:

1. **Minimalism** – one keyword per line, Markdown‑style readability.
2. **Determinism** – explicit waits + error codes, zero hidden retries.
3. **Portability** – runs on any Chromium, Firefox, or WebKit driver.
4. **Security & Observability** – sandboxed execution + JSON logs.

---

## Quick Example

```actiontxt
OPEN https://example.com
WAIT SELECTOR #login-form
FILL #username WITH "gaurav"
FILL #password WITH "hunter2"
CLICK #submit-button
WAIT URL CONTAINS "/dashboard"
```

Six lines → same effect as \~40 lines of Playwright or \~70 lines of Selenium.

---

## Full White Paper

<details>
<summary>Click to expand the complete specification & rationale (2,000 words)</summary>

### Abstract

The rise of AI agents navigating the web necessitates a standardized, lightweight, and deterministic instruction schema. Current methods using verbose APIs or complex scripts are brittle, opaque, and non‑portable. We introduce **action.txt**, a minimalistic, text‑based schema designed for simplicity, determinism, portability, and security. This white paper outlines motivations, compares existing approaches, defines a formal syntax and error semantics, describes practical use cases, addresses implementation considerations, and offers illustrative examples, positioning action.txt as a standard execution layer for the agentic web.

---

### 1  Introduction

As the web evolves toward supporting autonomous agents, there's an urgent need for standardized task instructions. The "agentic web"—where automated agents perform complex online tasks—requires:

* **Determinism:** consistent and predictable outcomes.
* **Portability:** platform and browser agnostic.
* **Simplicity:** human‑readable for transparency and ease of auditing.
* **Security:** safe execution in controlled environments.

`action.txt` addresses these needs by providing an explicit, declarative instruction schema tailored to human readability and agent interpretability.

---

### 2  Background and Related Work

Existing web automation frameworks like Selenium, Puppeteer, or Playwright rely on verbose code tightly coupled to browser APIs. Emerging agent frameworks (AutoGPT, LangChain) often employ complex JSON schemas or hard‑coded behaviours, complicating readability and maintainability.

| Feature                 | Selenium | Playwright | JSON DSLs (AutoGPT) | **action.txt** |
| ----------------------- | -------- | ---------- | ------------------- | -------------- |
| Human Readability       | ◑        | ◑          | ✗                   | **✓**          |
| Determinism             | ◑        | ◑          | ◑                   | **✓**          |
| Portability             | ◑        | ◑          | ✓                   | **✓**          |
| Security / Auditability | ◑        | ◑          | ✗                   | **✓**          |

Inspired by Gherkin’s simplicity and HTTPie's readability, action.txt simplifies task definitions through structured, Markdown‑like instructions.

---

### 3  Design Philosophy

*Minimalism* · *Predictability* · *Composability* · *Security & Observability*

```actiontxt
OPEN https://example.com
WAIT SELECTOR #login-form
FILL #username WITH "gaurav"
FILL #password WITH "hunter2"
CLICK #submit-button
WAIT URL CONTAINS "/dashboard"
```

---

### 4  Formal Syntax Specification (EBNF)

```ebnf
command         = keyword , arguments ;
keyword         = "OPEN" | "CLICK" | "FILL" | "WAIT" | "IF" | "ELSE" | "LOOP" | "SET" ;
arguments       = url | selector | condition | variable_assignment ;
url             = "http" , { character } ;
selector        = "#" , identifier ;
condition       = "URL CONTAINS" , string | "SELECTOR" , selector ;
variable_assignment = identifier , "=" , value ;
value           = string | number ;
comment         = "#" , { character } ;
```

> **Full token grammar**, error codes, and exit semantics live in [`/spec/grammar.ebnf`](spec/grammar.ebnf).

---

### 5  Use Cases

1. **AI Agent Automation** – LLM agents scrape sites, book flights, fill CRMs.
2. **Robotic Process Automation** – non‑devs automate internal web dashboards.
3. **API Fallback** – browser‑based contingency when REST/GraphQL endpoints fail.

*Case study*: Yet to publish

---

### 6  Implementation Considerations

*Parser* → *Executor* → *State Engine* architecture.  Secure sandbox disables pop‑ups, downloads, and untrusted JS.  JSON log lines for every step enable CI diffing and replay.

---

### 7  Illustrative Examples

yet to publish

---

### 8  Roadmap

| Version  | Features                                                               | ETA               |
| -------- | ---------------------------------------------------------------------- | ----------------- |
| **v0.1** | `OPEN CLICK FILL WAIT` keywords, reference parser CLI                  | **✅ Released**    |
| **v0.2** | Variables, `IF/ELSE`, `LOOP`, exit codes, log spec                     | **WIP (Q3 2025)** |
| **v1.0** | Stable grammar, browser extension, VS Code plug‑in, governance charter | Q4 2025           |

---

### 9  References

SeleniumHQ • Playwright • Gherkin • AutoGPT • LangChain • OpenAI Function‑Calling

</details>

---

## Roadmap

yet to publish

---


## Contributing

1. Fork → create feature branch.
2. Add / modify `.action.txt` or spec docs.
3. Run `npm test` to lint & validate.
4. Submit a pull request.  All PRs require one approving review + green CI.

See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the full guidelines and RFC template.

---

## License

`action.txt` reference specification and tooling are licensed under the [MIT License](LICENSE).
