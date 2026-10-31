# Project Achilles — security review toolkit and training corpus

**Reusable security-review rules, Python checks, and a versioned training curriculum for evaluating evidence, authority, privacy, and consequential effects in agentic workflows.**

Project Achilles combines practical source-level checks with an explicit review methodology. It is useful when designing a delegated workflow, assessing whether a public artifact leaks private repository membership, or validating the integrity and recorded provenance of a security-review training package.

The repository is **source-visible, not open source**. Private evaluation and research permissions are bounded by [LICENSE](LICENSE); commercial use and redistribution require separate permission. See [COMMERCIAL_LICENSE.md](COMMERCIAL_LICENSE.md) and [NOTICE](NOTICE).

## What is included

| Component | Use it for | Implementation |
| --- | --- | --- |
| [Public-artifact guard](security/portfolio_publication_guard.py) | Detect explicit private inventory lists, unkeyed membership digests, and missing source-only effect fences | Importable Python functions returning structured findings |
| [Seven training package](training/roles/seven/v1.0.0/BOOTSTRAP.md) | Study evidence provenance, threat modeling, secure review, retry/recovery, authority, finding quality, and continuity | Versioned modules and qualification materials with a [manifest](training/roles/seven/v1.0.0/TRAINING_MANIFEST.json) |
| [Checkpoint verifier](training/roles/seven/tools/training_checkpoint.py) | Check package file hashes, source Git binding, and structural training-record requirements | Dependency-free Python CLI; see [checkpoint guide](training/roles/seven/CHECKPOINT.md) |
| [Execution Protocol V2](PROTOCOL_V2_CURRENT.md) | Classify the **next actual effect** and avoid both unjustified escalation and authorization bypass | Current governance guidance, not an executable authority service |
| [Security review example](security/PROJECT_RUNNER_PORTFOLIO_WAVE_V1_REVIEW.md) | Understand source-only portfolio publication risks and explicit residual findings | Historical, scope-limited review record |

These are distinct artifacts. The frozen training package records historical training criteria; the current V2 protocol governs how the review method is interpreted for present work. Neither one grants provider credentials or host permissions.

## Quick start

Requires Python 3 with its standard library and Git for source-binding verification. The checks in this repository do not declare third-party Python runtime dependencies.

```bash
git clone https://github.com/thebrazenbeard/achilles.git
cd achilles

# Compile the Python sources and run the repository's unittest suite.
python3 -m compileall -q training tests security
python3 -m unittest discover -s tests -p 'test_*.py' -v

# Verify the frozen Seven package's manifest, module order, and file hashes.
python3 training/roles/seven/tools/training_checkpoint.py verify-package \
  training/roles/seven/v1.0.0
```

You can also check the package's separate checksum ledger:

```bash
cd training/roles/seven/v1.0.0
sha256sum -c SHA256SUMS.txt
```

The checksum command requires `sha256sum` (common on Linux). Use an equivalent SHA-256 verification tool on other systems.

## Use the public-artifact privacy guard

The guard examines caller-supplied Python data structures; it does not discover private repositories or authorize publication. For example:

```python
from security.portfolio_publication_guard import find_public_private_inventory_leaks

publication = {
    "private_inventory": {"count": 3},
    "private_repositories": ["example/hidden-project"],
}

findings = find_public_private_inventory_leaks(publication)
for finding in findings:
    print(f"{finding.code}: {finding.path}")
```

The example reports `PRIVATE_IDENTIFIER_LIST`. The guard also exposes `review_source_only_wave_policy(payload)` to inspect whether a proposed **source-only** wave retains its declared limits on merges, deployments, credential changes, and destructive cleanup. These are bounded structural checks, not complete data-loss-prevention, policy enforcement, or proof of safe execution.

## Verify a training checkpoint

The [checkpoint tool](training/roles/seven/tools/training_checkpoint.py) has three commands:

- `verify-package`: validate the versioned manifest, referenced files, and ordering.
- `init-checkpoint`: create a checkpoint skeleton after validating exact source-repository, commit, and path bindings.
- `verify-checkpoint`: check the filled checkpoint against structural qualification requirements.

See the [checkpoint guide](training/roles/seven/CHECKPOINT.md) for complete arguments and an example using an exact Git commit SHA. A checkpoint should refer to the actual verified source commit, not a guessed or moving branch name.

**Important:** A mechanical `BASE_READY_MECHANICAL_GATE_PASS` does not mean a model passed a substantive security exam. Qualification still requires real exercise evidence and evaluator findings. A training package or role name is not an identity proof or operational grant.

## Review and authority boundaries

[Protocol V2](PROTOCOL_V2_CURRENT.md) distinguishes:

1. Read-only inspection.
2. Assigned isolated or reversible work.
3. Shared mutable work that needs real writer-collision control.
4. Consequential or protected effects that need applicable authority.

The appropriate response follows the **actual next effect**, not the most dangerous imaginable downstream operation. Writer leases prevent conflicting mutations; they do not generate consent or authorization.

Read `PROTOCOL_V2_CURRENT.md` before applying older doctrine to current work. The **older permission/lease exercises do not override** the current V2 effect-class guidance. The historical training package remains preserved as evidence of its own version.

The current repository does **not** supply a deployed authorization service, cryptographically host-bound provider identity, credential broker, independent human reviewer, automatic repository dispatcher, or live-host attestation. Passing source tests only supports their explicit test scope.

The historical [workspace-startup safety checks](tests/test_workspace_startup_safety.py) detect known tracked source vectors; they do not establish that any installed editor, extension, old checkout, or host is clean.

## Validation and contribution

- [Regression tests](tests/) cover protocol currentness, training-checkpoint integrity, known workspace-startup hazards, and public-artifact policy checks.
- [GitHub verification workflow](.github/workflows/verify.yml) runs Python compilation, `unittest`, manifest verification, and the checksum ledger on supported events.
- [Workspace startup workflow](.github/workflows/workspace-startup-safety.yml) checks the tracked editor-task hazard.

Read [CONTRIBUTING.md](CONTRIBUTING.md) and [CLA.md](CLA.md) before opening a pull request. The repository requires an express contributor-license acknowledgment in the PR description; an agent cannot make that legal representation for its operator. The proprietary [LICENSE](LICENSE) remains applicable even when the repository is publicly readable.
