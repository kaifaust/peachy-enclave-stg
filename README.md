# peachy-enclave-stg

**STAGING.** Public Tinfoil Container configuration for the *staging* assistant
enclave of [Peachy](https://pchy.app).

> ## Nothing here describes production.
>
> Production's enclave configuration lives in
> **[kaifaust/peachy-enclave](https://github.com/kaifaust/peachy-enclave)**, and
> that is the repo a client verifies against. A measurement published here
> describes a throwaway enclave running against a throwaway database. It is not
> a claim about anything a person's messages ever touched, and it must never be
> read as one.

## Why this is a separate repository, and not a tag series

Tinfoil measures `tinfoil-config.yml` at each tag and publishes the measurement
to Sigstore. Verifiers — including Peachy's server, the `pchy.app/verify` page
in every visitor's browser, and the iOS and macOS apps — read
**`releases/latest`** from a repository, *not* the tag a running container was
created from. `tinfoil container create` also defaults to
`--promote-release true`, which moves that pointer.

So a staging release cut into the production repository would retarget every
one of those verifiers at a staging measurement, and they would all report that
production's live enclave does not match. A `stg-v*` tag series inside the
production repo prevents none of that, because none of those readers name a
tag.

One pointer per repository is the whole reason this repository exists. There is
no pointer the two share.

## Layout

| Path | What it is |
|---|---|
| `tinfoil-config.yml` | The staging configuration measured at the tag being deployed. |
| `.github/workflows/` | The two release workflows, verbatim from `tinfoilsh/tinfoil-containers-template`, the same copies `peachy-enclave` carries. |

The source of truth for this file is `deploy/tinfoil/tinfoil-config.stg.yml` in
the private Peachy repo; the release step copies it here and substitutes the
real image digest. See `docs/staging.md` there for the design, the costs and
the teardown.

## Tags

Staging image and release tags are `stg-v0.0.x`. Production's are `v0.1.x`, in
the other repository. The prefixes differ so that a digest read off a build
job's summary cannot be pasted into the wrong release by muscle memory.

## Everything here is public by construction

The resource shape, the egress allowlist, the *names* (never the values) of
secrets, and the image digest. That is true of the production repo too, and it
is the point of both.
