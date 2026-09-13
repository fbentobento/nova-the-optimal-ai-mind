# Quickstart from source

This gets you from `git clone` to a locally built, verified Nova Free 3.1.5 customer package. It does not itself install Nova into a Codex or Claude host — [`START-HERE.md`](../START-HERE.md) covers that once you have the archive.

Requires Python 3.10 or newer already on your `PATH`.

## Install runtime dependency

The test suite imports `jsonschema`, which is not vendored. Install it once:

    python3 -m pip install jsonschema

## Qualify the source

Regenerate custody and run the deterministic checks, per [`docs/VERIFICATION.md`](VERIFICATION.md):

    python3 -B -X utf8 tools/generate_source_lock.py > /dev/null
    python3 -B -X utf8 -m unittest discover -s tests
    python3 -B -X utf8 tools/check_documentation.py
    python3 -B -X utf8 docs/check_site.py
    git diff --check

All five must be clean. `tools/generate_source_lock.py` writes `design/source-lock.json`; if it changes on your tree, that is a real drift you should record and commit before continuing, not paper over.

## Build and verify the package

    python3 -B -X utf8 tools/build_release.py --repo .
    python3 -B -X utf8 tools/verify_package.py dist/nova-the-optimal-ai-free-3.1.5

`build_release.py` writes the customer, Codex, and Claude-compatible archives (plus their `.sha256` sidecars) to `release/`. A PASS from `verify_package.py` confirms exact staged inventory, deterministic archive structure, per-file checksums, host parity, and rights-envelope custody.

## Hand off

Give the resulting `release/nova-the-optimal-ai-free-3.1.5.zip` to an installing agent on the target host and follow [`START-HERE.md`](../START-HERE.md).

## Evidence boundary

A package PASS establishes the exercised archive inventory, exact payload bytes, rights custody, per-file checksums, deterministic structure, host parity, source binding, and truthful not-published state. It does not establish fresh-host marketplace acceptance, installation, discovery, enabled state, restart behavior, model attention, routing quality, live tools, external-service behavior, publication, or customer outcomes.
