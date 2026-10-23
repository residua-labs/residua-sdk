# residua-sdk

> **Release status (2026-10-23):** pre-release research mirror. It is **not** part of the first Residua release (bootstrap 1.0 + R-CIC) and is not being expanded until it is closed. See [residua-labs/Residua](https://github.com/residua-labs/Residua) for release status.

The host-language SDK for **embedding Residua** in other languages: TS, Python (via Wasm), and Rust.

The design lives in the main repository:
- ADR draft *frontend trust tiers*, §5a (embedding mode) and §5b (one proved lowering, with interpreters as oracles): minustwo/residua#7881;
- plan *Agent Proof Track*, work packages P6.0–P6.3: minustwo/residua#7929.

## Why a separate repository

The main repository admits no new host-language code: nf-admit refuses `.py`, `.sh` and `.js`/`.ts` growth, and `rust/` is a frozen substrate zone. The host-neutral half stays in the main repository:
- the layout specification;
- the scm codec round-trip checker;
- the receipt verifier specification.

This repository holds only the per-host wrappers, generated from that layout specification.

## Contract (summary)

- **Codecs.** Each host codec is generated from the single layout spec. Its round-trip is tested against the main repository's scm checker.
- **Calls.** A call returns `(output, receipt)`. The receipt binds the module `q`, the input `q`, the output `q`, the substrate and the evidence kind.
- **Verifier.** One minimal local receipt verifier per host, ported, not reimplemented in spirit.
- **Backends.** Calls are backed by the existing interpreters until the bootstrap's proved scm→Wasm lowering (E6.1) is usable. Then the API stays the same and the production path switches to the lowered Wasm, with the interpreters kept as differential oracles.

Status: empty scaffold. `closure_claim = 0`.
