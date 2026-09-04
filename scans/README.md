# Scan records

One folder per released version with the antivirus verdicts, engine and
signature versions, the checksum verification, the build attestation
(release manifest and package audit) and the unit-test summary for that
exact file.

GitHub Actions logs expire; these pages do not. The live workflow that
produces them is [`security-scan`](../.github/workflows/security-scan.yml),
which also runs on every push of a new exe. What the program does and how to
verify it with your own tools is in [SECURITY.md](../SECURITY.md).
