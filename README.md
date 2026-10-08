<div align="center">

# Roro

**I build security tooling for the AI supply chain.**

Mostly Go. Mostly the unglamorous parts: parsers that eat untrusted bytes,
evidence that survives serialization, and output a machine downstream can actually trust.

[![AIROM](https://img.shields.io/badge/AIROM-AI_Bill_of_Materials-2563EB?style=for-the-badge&logo=go&logoColor=white)](https://github.com/airomhq/airom)
[![airom.dev](https://img.shields.io/badge/airom.dev-visit-0F172A?style=for-the-badge)](https://airom.dev)
[![PyPI](https://img.shields.io/pypi/v/airom?style=for-the-badge&label=pip%20install%20airom&color=3B82F6)](https://pypi.org/project/airom/)

</div>

---

## What I'm building

### [AIROM](https://github.com/airomhq/airom) — the AI bill of materials that shows its work

An open-source **AIBOM scanner**. Point it at a repo, image, or Kubernetes workload and it
returns every AI component inside: models, prompts, datasets, embeddings, vector databases,
frameworks, serving infrastructure.

The part that makes it different is boring to say and hard to build: **every finding carries
the `file:line` it was seen at**, the detector that found it, and the arithmetic behind its
confidence score. When an auditor asks *"why does your AIBOM say `gpt-4.1`?"*, there is an
answer, and it is in the document.

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

<table>
<tr><td><b>Scale</b></td><td>~59k lines of Go · 114 test files · 29 releases · 69 rule packs · 9 languages</td></tr>
<tr><td><b>Standards</b></td><td>CycloneDX 1.6/1.7 · SPDX 3.0.1 · SARIF 2.1.0 · OpenVEX · NIST AI RMF · OWASP Agentic</td></tr>
<tr><td><b>Ships as</b></td><td>One static <code>CGO_ENABLED=0</code> binary, a Python SDK, and a signed rule-update channel</td></tr>
<tr><td><b>Supply chain</b></td><td>Keyless-cosign-signed releases · ed25519-signed rule bundles · reproducible builds</td></tr>
</table>

---

## How I work on it

**Parsers are hostile input, so they get fuzzed.** Every binary header parser — GGUF,
safetensors, ONNX, PyTorch, SavedModel, TFLite, HDF5, TensorRT — runs under a fuzz campaign in
CI and must return an error, never panic. Model weights are identified by magic bytes and
bounded header reads. Nothing is ever loaded, deserialized, or executed.

**Unknown is not the same as safe.** A version that could not be resolved stays empty instead
of being guessed. A model outside the lifecycle catalog gets no claim rather than a quiet
"supported". Every scan emits an assurance account stating what it could *not* prove.

**A fix without a failing test is a guess.** When I fix something, I first make the test fail
without the fix. That found a traversal guard that let an archive entry named `..` resolve to
its parent, a rule pack shipping twice so its findings counted double, and a lexer that had
been taking untrusted bytes for months with nothing fuzzing it.

**The build is the gate.** 7 CI workflows: race tests, cross-compilation to 6 targets, fuzz
smoke, perf gates with an RSS ceiling, CodeQL, govulncheck, and a benchmark gate that fails on
detection regression.

---

## The ecosystem

| Repo | What it is |
|---|---|
| [**airom**](https://github.com/airomhq/airom) | The scanner. Go, Apache-2.0. |
| [**airom-rules**](https://github.com/airomhq/airom-rules) | Signed rule channel — new providers reach users without a new binary. |
| [**airom-bench**](https://github.com/airomhq/airom-bench) | Public benchmark corpus. Detection is measured, not asserted. |
| [**airom-web**](https://github.com/airomhq/airom-web) | [airom.dev](https://airom.dev) and [docs.airom.dev](https://docs.airom.dev). |

---

## Toolbox

![Go](https://img.shields.io/badge/Go-00ADD8?style=flat-square&logo=go&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white)
![Java](https://img.shields.io/badge/Java-ED8B00?style=flat-square&logo=openjdk&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?style=flat-square&logo=docker&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub_Actions-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![Jenkins](https://img.shields.io/badge/Jenkins-D24939?style=flat-square&logo=jenkins&logoColor=white)

---

<div align="center">

<img height="165" src="https://github-readme-stats.vercel.app/api?username=Roro1727&show_icons=true&include_all_commits=true&count_private=true&hide_border=true&title_color=3B82F6&icon_color=3B82F6&bg_color=0D1117&text_color=C9D1D9" alt="stats" />
<img height="165" src="https://github-readme-stats.vercel.app/api/top-langs/?username=Roro1727&layout=compact&hide_border=true&title_color=3B82F6&bg_color=0D1117&text_color=C9D1D9&langs_count=6" alt="languages" />

</div>

---

<div align="center">

**Working on AI supply chain security. Always up for talking about evidence models, parser safety, or why your SBOM has no idea what an embedding is.**

[![airom.dev](https://img.shields.io/badge/airom.dev-0F172A?style=flat-square&logo=firefox&logoColor=white)](https://airom.dev)
[![Issues](https://img.shields.io/badge/Reach_me-via_GitHub_issues-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/airomhq/airom/issues)

</div>
