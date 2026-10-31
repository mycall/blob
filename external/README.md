# External assets

Keep large datasets, downloaded archives, prebuilt binaries, and external build
outputs outside Git. Track the information needed to obtain or reproduce them.

## Layout

| Path | Git policy | Contents |
| --- | --- | --- |
| `sources/` | Tracked | Small source and build manifests, one per asset/version. |
| `downloads/` | Ignored | Downloaded archives and external source checkouts. |
| `datasets/` | Ignored | Extracted or prepared datasets. |
| `binaries/` | Ignored | Downloaded or locally built external executables and libraries. |
| `build/` | Ignored | External build trees and intermediate files. |

Create payload directories as needed. Use an asset name and immutable version or
content hash in each payload's path so different versions can coexist. Very large
assets may live on another volume or in an artifact store; record their durable
location in the manifest and keep machine-local path overrides out of Git.

For generated experiment outputs and retention, follow the
[run-record policy](../docs/guides/autonomous-research.md#records-and-retention).
This document owns external inputs and their reconstruction.

## Source manifests

Use a small JSON or YAML file under `sources/`. No manifest loader or downloader is
implemented yet. Record enough information for another machine to reconstruct the
asset, including:

- Asset identifier, kind, upstream project or publisher, and immutable version.
- Retrieval URL or documented retrieval procedure, including access requirements.
  Never include credentials or signed download URLs containing secrets.
- License and applicable access or redistribution conditions.
- SHA-256 digest and byte size of each downloaded file. For a dataset with many
  files, identify a checksummed file inventory or a reproducible archive.
- Expected local path relative to this directory, or an external storage reference.
- Extraction, preparation, or build commands and their parameters.
- For built binaries: source revision and patches, pinned build dependencies or
  container digest, target platform, compiler options, and output hashes.
- For transformed datasets: input hashes, transformation code revision, parameters,
  and output hashes.

Keep an existing asset version's manifest stable once a recorded experiment refers
to it. Create a new version for changed inputs or preparation steps. Use explicit
amendments for corrections to historical provenance.

## Using an asset

1. Record its source and expected identity in a manifest.
2. Download or build it into the appropriate ignored directory.
3. Verify its digest before using it; verify extracted contents when an inventory
   is supplied. A mutable URL or a filename alone does not establish identity.
4. Reference the manifest and verified content hashes from experiment run records.
5. Preserve essential inputs and outputs in durable storage before treating a run
   as reproducible. If access restrictions prevent preservation or sharing, record
   that limitation explicitly.

Small, intentionally versioned test fixtures belong in `../corpus/` or
`../contracts/conformance/`. Ignore rules are scoped to payload directories rather
than file extensions so those fixtures and result manifests remain trackable.
