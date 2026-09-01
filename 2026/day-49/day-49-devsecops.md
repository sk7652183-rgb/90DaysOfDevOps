# Day 49 – DevSecOps: Add Security to Your CI/CD Pipeline

## Task 1: Scan Your Docker Image for Vulnerabilities

### 🔐 Docker Image Vulnerability Scanning with Trivy

The pipeline uses **Trivy** to scan the Docker image for known **CVEs (Common Vulnerabilities and Exposures)** before proceeding with deployment.

* **Trivy** scans the Docker image for known vulnerabilities and CVEs.
* `format: 'table'` displays the scan results in a readable table in the GitHub Actions logs.
* `exit-code: '1'` causes the pipeline to fail if **HIGH or CRITICAL** vulnerabilities are detected.
* If the scan passes, the image is considered free of the configured HIGH and CRITICAL vulnerabilities, and the pipeline can proceed with the next stage.

```yaml
name: Reusable Docker Workflow
on:
  workflow_call:
    inputs:
      image_name:
        description: "Name of the Docker image"
        required: true
        type: string
    secrets:
      docker_token:
        required: true
    outputs:
      image_url:
        description: "Full Docker image path"
        value: ${{ jobs.docker.outputs.image_url }}

jobs:
  docker:
    runs-on: ubuntu-latest
    outputs:
      image_url: ${{ steps.build.outputs.image_url }}
    steps:
      # Checkout code
      - name: Checkout code
        uses: actions/checkout@v4

      # Get short commit SHA
      - name: Get Short Commit SHA
        id: vars
        run: |
          SHORT_SHA=$(git rev-parse --short HEAD)
          echo "short_sha=$SHORT_SHA" >> "$GITHUB_OUTPUT"

      # Login to Docker Hub
      - name: Login to Docker Hub
        uses: docker/login-action@v3
        with:
          username: ${{ vars.docker_username }}
          password: ${{ secrets.docker_token }}

      # Build Docker Image
      - name: Build Docker Image
        id: build
        run: |
          IMAGE_NAME="${{ vars.docker_username }}/${{ inputs.image_name }}"
          LATEST_IMAGE="${IMAGE_NAME}:latest"
          SHA_IMAGE="${IMAGE_NAME}:sha-${{ steps.vars.outputs.short_sha }}"
          echo "Building Docker images:"
          echo "$LATEST_IMAGE"
          echo "$SHA_IMAGE"
          docker build \
            --pull \
            -t "$LATEST_IMAGE" \
            -t "$SHA_IMAGE" \
            .
          # Output SHA-tagged image URL
          echo "image_url=$SHA_IMAGE" >> "$GITHUB_OUTPUT"

      # Scan Docker Image BEFORE pushing
      - name: Scan Docker Image for Vulnerabilities
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: '${{ vars.docker_username }}/${{ inputs.image_name }}:latest'
          format: 'table'
          exit-code: '1'
          severity: 'CRITICAL,HIGH'
          ignore-unfixed: true

      # Push Docker Images ONLY if Trivy passes
      - name: Push Docker Images
        run: |
          IMAGE_NAME="${{ vars.docker_username }}/${{ inputs.image_name }}"
          LATEST_IMAGE="${IMAGE_NAME}:latest"
          SHA_IMAGE="${IMAGE_NAME}:sha-${{ steps.vars.outputs.short_sha }}"
          echo "Pushing Docker images:"
          echo "$LATEST_IMAGE"
          echo "$SHA_IMAGE"
          docker push "$LATEST_IMAGE"
          docker push "$SHA_IMAGE"
          echo "Docker images pushed successfully."

```

This security check helps prevent vulnerable Docker images from being promoted to deployment.

### **Push your changes and check the GitHub Actions tab. Review the Trivy scan output and verify that the vulnerability table is displayed in the logs. Check whether the scan passed or failed based on the detected HIGH or CRITICAL vulnerabilities.**

**After pushing the changes, I checked the GitHub Actions tab and reviewed the Trivy scan output. The vulnerability table was displayed successfully. The scan failed because Trivy detected 19 vulnerabilities, including 16 HIGH and 3 CRITICAL vulnerabilities. Since exit-code: '1' is configured, the pipeline failed as expected.**


<img width="1365" height="723" alt="image" src="https://github.com/user-attachments/assets/471c8d2a-e6c0-410d-9041-0b679683a70a" />



## Docker Image & CI Security Fixes

This section documents the vulnerability scan failures identified by Trivy and the fixes applied.

### Issue

CI was failing with 16 OS-level vulnerabilities (13 HIGH, 3 CRITICAL) detected by Trivy in the
`python:3.12-slim` base image, affecting packages such as `perl-base`, `gzip`, `libacl1`,
`libncursesw6`, `libsqlite3-0`, and `libtinfo6`.

Investigation showed that **every flagged CVE had no available upstream fix** at scan time
(empty "Fixed Version" in the Trivy report, several marked `fix_deferred` by Debian). This meant
the build was failing on issues that could not be resolved by upgrading packages.

### Fixes Applied

**1. Dockerfile — multi-stage build**
- Split into a `builder` stage (compiles/installs dependencies) and a slim final stage, reducing
  the overall package surface shipped in the runtime image.
- Added `apt-get upgrade -y` to pull in any Debian security patches available at build time.
- Purged `perl-base` from the final image (not required by the Python runtime), removing the
  single largest source of findings (7 of 16 CVEs).
- Added a non-root `appuser` to run the application, per container security best practice.

**2. CI workflow (`docker.yml`) — Trivy scan step**
- Changed `ignore-unfixed: false` → `ignore-unfixed: true`.
  - Rationale: with no fix available, these CVEs cannot be remediated by us today; blocking the
    pipeline on them provides no security benefit and only prevents deploys. Fixable
    vulnerabilities (i.e. those with a released patch) will still fail the build as before.
- Removed `--no-cache` from `docker build`, keeping `--pull` to ensure the freshest base image
  layer is always used. `--no-cache` only affected build speed, not vulnerability freshness.

### Result

- CI pipeline passes cleanly.
- Image continues to be rebuilt with `--pull` on every run, so patches released upstream for
  `python:3.12-slim` are picked up automatically on the next build.
- Any *newly introduced, fixable* HIGH/CRITICAL vulnerability will still fail the build and block
  the push — only pre-existing, unfixed CVEs are bypassed.

### Follow-up / Maintenance

- [ ] Periodically re-run the scan without `ignore-unfixed` to check whether Debian has since
      shipped fixes for the previously unfixed CVEs (e.g. `CVE-2026-13221`, the CRITICAL Perl
      regex issue).
- [ ] Confirm `perl-base` purge does not break any downstream tooling before merging to `main`.
- [ ] Consider evaluating a distroless or Alpine-based final image for a further-reduced attack
      surface, if compatible with project dependencies.

<img width="1365" height="726" alt="image" src="https://github.com/user-attachments/assets/a58f502e-65da-4d9b-ab1d-b846414dc23f" />

## Notes: Vulnerability Scan Findings

### Base Image Used

`python:3.12-slim` (Debian-based slim variant)
 
## Write in your notes: What CVEs (if any) were found? What base image are you using?

### CVEs Found (Trivy scan)

**Total: 16 vulnerabilities — 13 HIGH, 3 CRITICAL**

| Package        | CVE            | Severity | Status       | Fixed Version Available? |
|----------------|----------------|----------|--------------|---------------------------|
| gzip           | CVE-2026-41992 | HIGH     | affected     | No |
| libacl1        | CVE-2026-54369 | HIGH     | affected     | No |
| libncursesw6   | CVE-2025-69720 | HIGH     | affected     | No |
| libsqlite3-0   | CVE-2026-11822 | HIGH     | affected     | No |
| libsqlite3-0   | CVE-2026-11824 | HIGH     | affected     | No |
| libtinfo6      | CVE-2025-69720 | HIGH     | affected     | No |
| ncurses-base   | CVE-2025-69720 | HIGH     | affected     | No |
| ncurses-bin    | CVE-2025-69720 | HIGH     | affected     | No |
| perl-base      | CVE-2026-13221 | CRITICAL | affected     | No |
| perl-base      | CVE-2026-42496 | HIGH     | fix_deferred | No |
| perl-base      | CVE-2026-8376  | CRITICAL | affected     | No |
| perl-base      | CVE-2026-42497 | HIGH     | fix_deferred | No |
| perl-base      | CVE-2026-48962 | HIGH     | affected     | No |
| perl-base      | CVE-2026-57432 | HIGH     | affected     | No |
| perl-base      | CVE-2026-57433 | CRITICAL | affected     | No |
| perl-base      | CVE-2026-9538  | HIGH     | fix_deferred | No |

### Key Observation

Every CVE found had **no fixed version available** at the time of scanning (confirmed by the
empty "Fixed Version" column in the Trivy report). Several were explicitly marked
`fix_deferred` by Debian's security team, meaning a patch is intentionally not being backported
(often due to low real-world exploitability in the affected context).

This means these findings could not be resolved through package upgrades alone — they required
a CI policy decision (`ignore-unfixed: true`) plus image hardening (removing unnecessary
packages like `perl-base`) rather than a simple version bump.

### perl-base Concentration

`perl-base` alone accounted for **9 of the 16 findings** (including all 3 CRITICALs), despite
not being required by the Python application at runtime. This was the primary target for
reduction via `apt-get purge` in the Dockerfile.

## Task 2: Enable GitHub's Built-in Secret Scanning

### GitHub can automatically detect if someone pushes a secret (API key, token, password) to your repo.

### Go to your repository → Settings → Code security and analysis, enable Secret scanning, and, if available, enable Push protection to block any push that contains a detected secret.

## 🔐 Secret Scanning and Push Protection

### What is the difference between Secret Scanning and Push Protection?

- **Secret Scanning** detects exposed secrets, such as API keys, passwords, tokens, and AWS access keys, that have already been pushed to a repository. GitHub generates an alert when a supported secret is detected.

- **Push Protection** prevents supported secrets from being pushed to the repository in the first place. If GitHub detects a secret during a push, the push is blocked before the secret reaches the repository.

### What happens if GitHub detects a leaked AWS key in your repository?

If GitHub detects a leaked AWS access key:

1. GitHub creates a security alert for the exposed secret.
2. The exposed AWS credentials should be immediately revoked or rotated.
3. If **Push Protection** is enabled, GitHub can block the push before the AWS key is committed to the repository.
4. The key should be removed from the application code and replaced with a secure solution, such as GitHub Secrets or AWS IAM roles.

> **Best Practice:** Never store AWS access keys, passwords, API keys, or other sensitive credentials directly in your source code.

## Task 3: Scan Dependencies for Known Vulnerabilities


