<div align="center">

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:020617,55:1E3A8A,100:2563EB&height=8&section=header" width="100%" alt="" />

# Rohan Patel

### AI supply chain security · Go · Open source

**I build security tooling for the AI supply chain.**
Creator of [AIROM](https://github.com/airomhq/airom), an open-source AIBOM scanner where every finding carries the `file:line` it came from.

<a href="https://airom.dev"><img src="https://img.shields.io/badge/airom.dev-020617?style=for-the-badge&logo=googlechrome&logoColor=white" /></a>
<a href="https://www.linkedin.com/in/rohanpatel8727/"><img src="https://img.shields.io/badge/LinkedIn-1E3A8A?style=for-the-badge&logo=linkedin&logoColor=white" /></a>
<a href="https://github.com/airomhq/airom"><img src="https://img.shields.io/badge/AIROM-2563EB?style=for-the-badge&logo=go&logoColor=white" /></a>
<a href="https://pypi.org/project/airom/"><img src="https://img.shields.io/pypi/v/airom?style=for-the-badge&label=pip%20install%20airom&color=3B82F6" /></a>
<a href="https://docs.airom.dev"><img src="https://img.shields.io/badge/Docs-60A5FA?style=for-the-badge&logo=readthedocs&logoColor=white" /></a>

<img src="https://capsule-render.vercel.app/api?type=rect&color=0:2563EB,45:1E3A8A,100:020617&height=8&section=header" width="100%" alt="" />

</div>

---

### About

I build **AIROM**, an open-source AI Bill of Materials scanner. It finds the AI inside a
codebase — models, prompts, datasets, embeddings, vector databases, frameworks — and records
the evidence behind every single finding.

Most of my work is the unglamorous half of that: parsers that eat untrusted bytes without
trusting them, an evidence model that survives serialization into five formats, and a release
pipeline you can verify without taking my word for anything.

> Unknown is not the same as safe. A scanner that guesses is worse than one that says nothing.

---

### AIROM — the AI bill of materials that shows its work

```bash
pip install airom && airom scan .
```

```
┌──────────────────┬────────────────────────┬─────────┬──────────┬───────┬────────────────────┐
│ KIND             │ NAME                   │ VERSION │ PROVIDER │ CONF  │ LOCATION           │
├──────────────────┼────────────────────────┼─────────┼──────────┼───────┼────────────────────┤
│ hosted-llm       │ gpt-4.1                │ -       │ openai   │ 0.85  │ src/rag.py:15      │
│ embedding-model  │ text-embedding-3-large │ -       │ openai   │ 0.85  │ src/rag.py:6       │
│ vector-db        │ chroma                 │ 0.5.5   │ chroma   │ 0.985 │ requirements.txt:4 │
│ local-model-file │ tiny.gguf              │ -       │ local    │ 0.95  │ models/tiny.gguf   │
└──────────────────┴────────────────────────┴─────────┴──────────┴───────┴────────────────────┘
```

That `LOCATION` column is the whole point. When an auditor asks *"why does your AIBOM say
`gpt-4.1`?"*, there is an answer and it is in the document: the file, the line, the detector
that fired, and the arithmetic behind the confidence score.

<table>
<tr>
<td width="50%" valign="top">

#### What it is

~59k lines of Go, 114 test files, 29 releases. Ships as one static `CGO_ENABLED=0` binary, a
Python SDK on PyPI, and a signed rule channel.

Detects across **9 languages**, from manifests, lockfiles, installed metadata, binary model
headers, and even frozen PyInstaller archives.

Emits **CycloneDX 1.6/1.7**, **SPDX 3.0.1**, **SARIF 2.1.0**, **OpenVEX**, JSON, YAML, and a
compliance view mapping NIST AI RMF and OWASP Agentic controls.

</td>
<td width="50%" valign="top">

#### How it is built

**Parsers are hostile input.** Every binary header parser — GGUF, safetensors, ONNX, PyTorch,
SavedModel, TFLite, HDF5, TensorRT — is fuzzed in CI and must return an error, never panic.
Weights are identified by magic bytes. Nothing is loaded or executed, ever.

**Releases are verifiable.** Keyless-cosign-signed, reproducible, checksummed. The rule-update
channel is ed25519-signed with rollback protection.

**The build is the gate.** Race tests, 6 cross-compile targets, fuzz campaigns, an RSS
ceiling, CodeQL, govulncheck, and a benchmark gate that fails on detection regression.

</td>
</tr>
</table>

---

### Things I shipped recently

<table>
<tr>
<td width="50%" valign="top">

**A traversal guard that let `..` through**
The check tested for `..` followed by a separator, so an entry named exactly `..` resolved to
the extraction root's **parent**. Not exploitable — it failed safe by accident rather than by
design. Replaced with `filepath.IsLocal` plus eight refusal cases.

**A rule pack that shipped twice**
Two packs, same keywords, different IDs. One line of Python produced **four** occurrences of
one component, feeding the confidence calculus corroboration that did not exist.

</td>
<td width="50%" valign="top">

**A lexer nothing ever fuzzed**
`allLangs` was commented "every language with a real lexer" and listed eight. The config table
had nine. SQL was the gap, and `.sql` files are scanned in a normal run. ~13M fuzz executions
later: no crash, and a guard now asserts the two lists agree.

**A schema that rejected its own output**
`tool.eolCatalog` was emitted for months, never declared in a schema that closes
`additionalProperties`. Nothing noticed, because the only test checked that the file parses.

</td>
</tr>
</table>

Each of those shipped with a test that fails without the fix. A fix without a failing test is a
guess.

---

### The ecosystem

| | |
|---|---|
| [**airom**](https://github.com/airomhq/airom) | The scanner. Go, Apache-2.0. |
| [**airom-rules**](https://github.com/airomhq/airom-rules) | Signed rule channel — new providers reach users without a new binary. |
| [**airom-bench**](https://github.com/airomhq/airom-bench) | Public benchmark corpus. Detection is measured, not asserted. |
| [**airom-web**](https://github.com/airomhq/airom-web) | [airom.dev](https://airom.dev) and [docs.airom.dev](https://docs.airom.dev). |

---

### Stack

<p>
<img src="https://img.shields.io/badge/Go-00ADD8?style=for-the-badge&logo=go&logoColor=white" />
<img src="https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white" />
<img src="https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white" />
<img src="https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white" />
<img src="https://img.shields.io/badge/Kubernetes-326CE5?style=for-the-badge&logo=kubernetes&logoColor=white" />
<img src="https://img.shields.io/badge/GitHub_Actions-2088FF?style=for-the-badge&logo=githubactions&logoColor=white" />
<img src="https://img.shields.io/badge/CycloneDX-000000?style=for-the-badge&logo=cyclonedx&logoColor=white" />
<img src="https://img.shields.io/badge/Sigstore-2F2F2F?style=for-the-badge&logo=sigstore&logoColor=white" />
</p>

---

<div align="center">

<a href="https://github.com/airomhq/airom"><img src="https://github-readme-stats.vercel.app/api/pin/?username=airomhq&repo=airom&hide_border=true&title_color=3B82F6&icon_color=3B82F6&bg_color=020617&text_color=C9D1D9" alt="airomhq/airom" /></a>
<a href="https://github.com/airomhq/airom-rules"><img src="https://github-readme-stats.vercel.app/api/pin/?username=airomhq&repo=airom-rules&hide_border=true&title_color=3B82F6&icon_color=3B82F6&bg_color=020617&text_color=C9D1D9" alt="airomhq/airom-rules" /></a>

<img height="160" src="https://github-readme-stats.vercel.app/api?username=Roro1727&show_icons=true&count_private=true&hide=stars&hide_border=true&title_color=3B82F6&icon_color=3B82F6&bg_color=020617&text_color=C9D1D9" alt="stats" />

<br/><br/>

**Happy to talk about evidence models, parser safety, or why your SBOM has no idea what an embedding is.**

<a href="https://www.linkedin.com/in/rohanpatel8727/"><img src="https://img.shields.io/badge/Let's_talk-LinkedIn-1E3A8A?style=for-the-badge&logo=linkedin&logoColor=white" /></a>

</div>
