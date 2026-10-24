# AlphaBravo Ghost image signing key

AlphaBravo publishes its Ghost container-image verification key here so customers can verify image signatures independently of our build infrastructure. This repository contains public material only.

## Key identity

| Field | Value |
| --- | --- |
| Publisher | AlphaBravo / [AlphaBravoCompany](https://github.com/AlphaBravoCompany) |
| Purpose | Verify Ghost container-image signatures and signed factory evidence |
| Public key | [cosign.pub](cosign.pub) |
| Key ID | `ghost-356d5ea015368ccc` |
| Algorithm | ECDSA P-256 |
| Published | 2026-10-24 UTC |
| Status | Published; factory activation and first release verification pending |
| Fingerprint format | SHA-256 of DER-encoded SubjectPublicKeyInfo |

```text
356d5ea015368cccebedca6e8da01925f991c58c681a486e29f2d7de4eb67931
```

This fingerprint hashes the decoded public key, not the PEM file's text. Older trial images used a different key; this key does not verify their signatures.

## Trust and download

Confirm the fingerprint with your established AlphaBravo contact before trusting this key. A fingerprint obtained only from the same repository as the key is not an independent trust check. Retain your approved key and pin a reviewed repository commit in automated deployments instead of automatically accepting future key changes.

Download the public key:

```sh
curl --fail --silent --show-error --location \
  https://raw.githubusercontent.com/AlphaBravoCompany/ghost-cosign/main/cosign.pub \
  --output ghost-cosign.pub
```

Display its DER fingerprint and compare it with your independently approved value:

```sh
openssl pkey -pubin -in ghost-cosign.pub -outform DER | openssl dgst -sha256
```

## Verify an image

Use a current Cosign client and the exact image digest from your release information. Authenticate to the registry separately if it requires credentials. Replace both placeholders below:

```sh
cosign verify \
  --key ghost-cosign.pub \
  --insecure-ignore-tlog \
  'registry.ghost-prod.alphabravo.io/ghost-patched-v2/<repository>@sha256:<digest>'
```

The factory currently publishes without a public transparency log. `--insecure-ignore-tlog` skips the transparency-log check; public-key signature and image-digest verification still apply. This provides no public transparency-log inclusion proof. See [Cosign verification documentation](https://docs.sigstore.dev/cosign/verifying/verify/).

A valid image signature establishes that the signing key approved that digest. It does not by itself prove zero vulnerabilities, validate VEX claims, or replace checking the image's SBOM, signed gate assessment and functional-test results. Verify those statements and their exact image/component bindings before applying your acceptance policy.

## Rotation and revocation

New identities will receive distinct public-key files and fingerprints. Preserve this key for historical verification; do not silently replace its contents. Record activation, retirement or revocation here and communicate trust changes through established customer channels. Consumers should review status updates and their own trust policy before accepting a retired or revoked key.

For fingerprint confirmation or suspected key compromise, contact your established AlphaBravo support contact. Never post private keys, passwords or registry credentials in this repository or its issues.
