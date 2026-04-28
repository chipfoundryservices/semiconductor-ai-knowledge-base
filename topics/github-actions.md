# GitHub Actions

**Keywords**: github actions, ci cd pipeline, continuous integration, workflow automation, devops, github workflow

---

**GitHub Actions** is **GitHub's native CI/CD and workflow automation platform that executes YAML-defined pipelines in response to repository events** — enabling teams to automate building, testing, deploying, and operating software without leaving the GitHub ecosystem, and now one of the most widely adopted CI/CD platforms globally with over 50 million repositories using it for automation.

**Core Concepts and Architecture**

GitHub Actions is built around five hierarchical concepts:

- **Workflow**: A YAML file stored in `.github/workflows/`. Defines the entire automation. A repository can have multiple workflows (e.g., `ci.yml`, `deploy.yml`, `nightly-train.yml`).
- **Event (Trigger)**: What causes the workflow to run. Common events include `push`, `pull_request`, `schedule` (cron), `workflow_dispatch` (manual trigger with parameters), `release`, and `repository_dispatch` (external API trigger).
- **Job**: A unit of work within a workflow. Jobs run in parallel by default. Jobs can be sequenced with `needs: [build]` dependencies. Each job runs on its own fresh runner VM.
- **Step**: A single command or action within a job. Steps run sequentially within a job and share the same VM environment.
- **Action**: A reusable, packaged step — either from the GitHub Marketplace (community-built) or defined inline. Called with `uses: owner/action-name@version`.

**YAML Structure Reference**

```yaml
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]
  schedule:
    - cron: '0 2 * * *'  # Nightly at 2am UTC

jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ['3.10', '3.11', '3.12']
    
    steps:
      - uses: actions/checkout@v4
      - name: Set up Python ${{ matrix.python-version }}
        uses: actions/setup-python@v5
        with:
          python-version: ${{ matrix.python-version }}
      - name: Install dependencies
        run: pip install -r requirements.txt
      - name: Run tests
        run: pytest tests/ --cov=src --cov-report=xml
      - name: Upload coverage
        uses: codecov/codecov-action@v4

  deploy:
    needs: test
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to production
        run: ./scripts/deploy.sh
        env:
          DEPLOY_KEY: ${{ secrets.DEPLOY_KEY }}
```

**Runners — Where Code Actually Executes**

Runners are the virtual machines that execute jobs. GitHub provides two types:

- **GitHub-hosted runners**: Managed VMs provisioned fresh for each job. Options: `ubuntu-latest` (Ubuntu 24.04), `windows-latest`, `macos-latest`. Include pre-installed toolchains (Python, Node, Java, Docker, etc.). Free tier: 2,000 minutes/month for public repos; private repos have limits by plan.
- **Self-hosted runners**: Your own machines (on-prem, cloud VM, bare metal). Register with `./config.sh --url https://github.com/org/repo --token TOKEN`. Essential for: GPU workloads (NVIDIA A100 for ML training), proprietary internal tools, compliance requirements, high-volume pipelines where GitHub-hosted minutes are expensive.
- **Larger GitHub-hosted runners**: Premium option — 4-core to 64-core VMs, including GPU runners (T4, A10G). Priced per-minute.

**Secrets and Security**

GitHub Actions has a multi-level secrets system:

- **Repository secrets**: `Settings → Secrets → Actions`. Accessed via `${{ secrets.MY_SECRET }}`. Never printed in logs.
- **Organization secrets**: Shared across multiple repositories, managed centrally.
- **Environment secrets**: Scoped to deployment environments (production, staging) with approval gates.
- **OpenID Connect (OIDC)**: The preferred method for cloud credentials. Instead of storing long-lived AWS/Azure/GCP credentials as secrets, configure OIDC trust, and GitHub Actions gets short-lived tokens on demand via `aws-actions/configure-aws-credentials@v4` with role assumption. Eliminates credential rotation burden and credential leak risk.

**Essential Marketplace Actions**

| Action | Purpose | Usage |
|--------|---------|-------|
| `actions/checkout@v4` | Clone repository | Required first step in virtually every job |
| `actions/setup-python@v5` | Install Python version | Matrix builds across Python versions |
| `actions/cache@v4` | Cache pip/npm/cargo deps | 2–5× faster builds |
| `docker/build-push-action@v6` | Build and push Docker images | Container-based deploys |
| `aws-actions/configure-aws-credentials@v4` | OIDC-based AWS auth | Preferred over static keys |
| `actions/upload-artifact@v4` | Share files between jobs | Pass build outputs to deploy job |
| `github/codeql-action@v3` | SAST security scanning | Free for public repos |

**AI and ML Specific Workflows**

GitHub Actions is increasingly used for ML pipelines:

- **Model evaluation CI**: On every PR, run a lightweight eval suite on a sample dataset. Flag PRs that regress accuracy below threshold. Prevents shipping model degradations.
- **Data validation**: Run Great Expectations or Pandera checks on data schema changes before merging.
- **Self-hosted GPU training**: Trigger fine-tuning jobs on self-hosted A100 runners on PRs to `model/` directory. Compare eval metrics against main-branch baseline, post results as PR comment via `actions/github-script`.
- **Docker image builds**: Automatically build and push training container images to ECR/GCR on merge to main.
- **Nightly benchmarks**: Schedule nightly inference speed benchmarks against production model, alert on regressions via Slack webhook.
- **DVC pipeline triggers**: Integrate with DVC (Data Version Control) to reproduce ML experiments on reproducible remote compute.

**Reusable Workflows and Composite Actions**

For teams with multiple repositories:

- **Reusable workflows**: A workflow file can be called from another workflow using `uses: org/repo/.github/workflows/shared-ci.yml@main`. Centralizes pipeline logic across repos.
- **Composite actions**: Package multiple steps into a single reusable action stored in a repo. Called like any marketplace action.
- **Organization-level workflow templates**: Templates that appear in the "Actions" tab for any new repo in the organization, enforcing consistent pipelines.

**Comparison with Other CI/CD Platforms**

| Platform | Strengths | Weaknesses |
|----------|-----------|-----------|
| **GitHub Actions** | GitHub-native, huge marketplace, generous free tier | Complex YAML for advanced cases, less flexible scheduling |
| **GitLab CI** | Tight repo integration, self-hosted easy | GitLab-only |
| **Jenkins** | Maximum flexibility, vast plugin ecosystem | Operational overhead, Groovy DSL complexity |
| **CircleCI** | Fast parallelism, Docker-first | Separate platform, per-org pricing |
| **ArgoCD** | GitOps for Kubernetes | Deployment-focused, not general CI |

GitHub Actions is the default choice for teams already on GitHub, particularly for AI/ML projects that benefit from tight integration with model repositories, dataset versioning (via LFS or DVC), and the ability to post automated metric reports directly on pull requests.

---

*Source: [ChipFoundryServices](https://www.chipfoundryservices.com/) — [Search this topic](https://www.chipfoundryservices.com/topic/github-actions) — [Ask CFSGPT](https://www.chipfoundryservices.com/chat/)*
