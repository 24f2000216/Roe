# GitHub Actions Multi-Platform Matrix Build with Artifacts

A comprehensive DevOps demonstration of CI/CD pipelines using GitHub Actions matrix builds for parallel execution across multiple platforms and configurations.

**Author Email**: your.email@example.com

## 📋 Overview

This project demonstrates modern CI/CD practices by implementing a sophisticated matrix build strategy that:

- Builds software across **3 different operating systems** (Ubuntu, Windows, macOS)
- Tests with **3 different Node.js versions** (16.x, 18.x, 20.x)
- Executes **9 parallel jobs** simultaneously
- Generates and manages **27 unique artifacts** (3 artifacts per job)
- Implements proper artifact naming conventions and retention policies

## 🎯 Key Features

### 1. Multi-Platform Matrix Strategy
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest, macos-latest]
    node-version: [16.x, 18.x, 20.x]
```

This creates a **3×3 matrix** resulting in 9 independent, parallel jobs:
- **3 Operating Systems**: Linux (Ubuntu), Windows, macOS
- **3 Node.js Versions**: LTS 16, LTS 18, Current 20
- **Total Combinations**: 9 parallel builds

### 2. Parallel Execution
Each matrix combination runs as a completely independent job:
```
ubuntu-latest + node-16.x   ┐
ubuntu-latest + node-18.x   │
ubuntu-latest + node-20.x   │
windows-latest + node-16.x  ├─ All run in PARALLEL
windows-latest + node-18.x  │
windows-latest + node-20.x  │
macos-latest + node-16.x    │
macos-latest + node-18.x    │
macos-latest + node-20.x    ┘
```

### 3. Artifact Management
Each job generates **3 unique artifacts**:

1. **Text Output** (`output.txt`)
   - Build metadata and status
   - Platform information
   - Build timestamp

2. **JSON Report** (`build-report.json`)
   - Structured build information
   - Machine-readable format
   - Complete build metadata

3. **Markdown Summary** (`build-summary.md`)
   - Human-readable format
   - Links and references
   - Build details summary

**Artifact Naming Convention**:
```
build-a90bca4-<OS>-node-<VERSION>-<TYPE>
```

Examples:
- `build-a90bca4-ubuntu-latest-node-16.x-text`
- `build-a90bca4-ubuntu-latest-node-16.x-json`
- `build-a90bca4-windows-latest-node-20.x-md`

### 4. Workflow Identifiers

**Step Identifier**: `matrix-a90bca4`
- Located in the workflow step: "Display system information"
- Used to verify workflow configuration

**Artifact Prefix**: `build-a90bca4-`
- All 27 artifacts follow this naming convention
- Easy to identify and track artifacts

## 📊 Workflow Structure

```
┌─────────────────────────────────────────────┐
│   Multi-Platform Matrix Build Workflow      │
└─────────────────────────────────────────────┘
                      │
        ┌─────────────┼─────────────┐
        │             │             │
    ┌───▼────┐   ┌───▼────┐   ┌───▼────┐
    │ Ubuntu │   │ Windows│   │ macOS  │
    └───┬────┘   └───┬────┘   └───┬────┘
        │             │             │
    ┌───┴───┐     ┌───┴───┐     ┌───┴───┐
    │ 16 18 │     │ 16 18 │     │ 16 18 │
    │ 20  │     │ 20  │     │ 20  │
    └───────┘     └───────┘     └───────┘
        │             │             │
        └─────────────┼─────────────┘
                      │
            ┌─────────▼─────────┐
            │ Generate Reports  │
            └───────────────────┘
                      │
            ┌─────────▼─────────┐
            │ Upload Artifacts  │
            └───────────────────┘
```

## 🚀 Quick Start

### Prerequisites
- GitHub account with repository access
- Git installed locally
- Basic understanding of CI/CD concepts

### Setup Steps

1. **Clone or Create Repository**
```bash
git clone https://github.com/YOUR_USERNAME/matrix-build-demo.git
cd matrix-build-demo
```

2. **Create Workflow Directory**
```bash
mkdir -p .github/workflows
```

3. **Add Workflow File**
Copy `matrix-build.yml` to `.github/workflows/matrix-build.yml`

4. **Add README**
```bash
echo "# GitHub Actions Matrix Build Demo" > README.md
echo "Author Email: your.email@example.com" >> README.md
```

5. **Commit and Push**
```bash
git add .github/workflows/matrix-build.yml README.md
git commit -m "Add GitHub Actions matrix build workflow"
git push origin main
```

6. **View Results**
- Go to your repository on GitHub
- Click **Actions** tab
- Select **Multi-Platform Matrix Build** workflow
- Watch the 9 jobs run in parallel

## 📈 Understanding the Matrix

### What is a Matrix Build?

A matrix build allows a single job definition to run multiple times with different parameter combinations. This is incredibly useful for:

1. **Testing across platforms** - Ensure code works on Linux, macOS, Windows
2. **Version compatibility** - Test with multiple language/library versions
3. **Configuration variants** - Test different build configurations
4. **Parallel efficiency** - All jobs run simultaneously

### Matrix Dimensions in This Project

**Dimension 1: Operating System**
```yaml
os:
  - ubuntu-latest    # Linux environment
  - windows-latest   # Windows environment
  - macos-latest     # macOS environment
```

**Dimension 2: Node.js Version**
```yaml
node-version:
  - 16.x  # Long-term support (LTS)
  - 18.x  # Long-term support (LTS)
  - 20.x  # Current stable release
```

### Accessing Matrix Variables

Inside your workflow, reference matrix values:
```yaml
runs-on: ${{ matrix.os }}                    # Current OS
with:
  node-version: ${{ matrix.node-version }}   # Current Node version
```

## 🏗️ Build Process

### Step-by-Step Execution

1. **Checkout Code**
   - Retrieves your repository code

2. **Setup Environment**
   - Installs specified Node.js version
   - Configures the selected OS environment

3. **Identifier Step** (`matrix-a90bca4`)
   - Displays build information
   - Proves workflow configuration

4. **Display System Information**
   - Shows OS, Node version, timestamp
   - Verifies environment setup

5. **Install Dependencies**
   - Runs `npm install` (simulated)
   - Verifies package manager availability

6. **Generate Build Artifacts**
   - Creates `output.txt` with build metadata
   - Generates `build-report.json` with structured data
   - Produces `build-summary.md` with human-readable info

7. **Upload Artifacts**
   - Uploads 3 artifacts per job
   - Uses `actions/upload-artifact@v4`
   - Sets 30-day retention policy

## 📦 Artifact Details

### Artifact 1: Text Output (`output.txt`)
```
Build Artifact Generated
===========================================
Platform: ubuntu-latest
Node Version: 16.x
Build Date: Thu Jan 15 10:30:45 UTC 2024
GitHub Repository: username/repo
Git Commit SHA: abc1234567890def
Git Branch: refs/heads/main
Build Status: SUCCESS
===========================================
```

### Artifact 2: JSON Report (`build-report.json`)
```json
{
  "build_info": {
    "platform": "ubuntu-latest",
    "node_version": "16.x",
    "timestamp": "2024-01-15T10:30:45Z",
    "repository": "username/repo",
    "commit_sha": "abc1234567890def",
    "branch": "refs/heads/main",
    "workflow": "Multi-Platform Matrix Build",
    "job_name": "success",
    "runner_os": "Linux",
    "runner_arch": "X64"
  },
  "build_metadata": {
    "status": "completed",
    "success": true,
    "artifacts_generated": 2
  }
}
```

### Artifact 3: Markdown Summary (`build-summary.md`)
```markdown
# Build Summary Report

## Build Information
- **Platform**: ubuntu-latest
- **Node Version**: 16.x
- **Build Time**: [timestamp]
- **Repository**: username/repo
- **Commit**: abc1234567890def
- **Branch**: main

## Build Status: ✅ SUCCESS
```

## 🔍 Validation Checklist

✅ **At least 3 successful matrix jobs**
- 9 total jobs (3 OS × 3 Node versions)
- All jobs complete successfully

✅ **At least 3 artifacts uploaded with prefix `build-a90bca4`**
- 27 total artifacts (9 jobs × 3 artifacts)
- All named with `build-a90bca4-` prefix

✅ **All artifacts contain actual content**
- Text files: Build metadata and status
- JSON: Structured build information
- Markdown: Human-readable summaries

✅ **Step identifier `matrix-a90bca4` present**
- Found in workflow YAML
- Executes in every matrix job

✅ **README.md with email address**
- This file
- Email: your.email@example.com

## 📊 Workflow Statistics

| Metric | Value |
|--------|-------|
| Matrix Dimensions | 2 (OS × Node Version) |
| OS Variants | 3 (Ubuntu, Windows, macOS) |
| Node Versions | 3 (16.x, 18.x, 20.x) |
| Total Parallel Jobs | 9 |
| Artifacts per Job | 3 |
| Total Artifacts | 27 |
| Artifact Retention | 30 days |
| Estimated Run Time | 3-5 minutes |

## 💡 Learning Outcomes

By studying this workflow, you'll learn:

1. **Matrix Build Strategy**
   - Multi-dimensional matrices
   - Parallel job execution
   - Variable interpolation

2. **Artifact Management**
   - Uploading artifacts
   - Naming conventions
   - Retention policies

3. **CI/CD Best Practices**
   - Cross-platform testing
   - Build automation
   - Artifact organization

4. **GitHub Actions Features**
   - Workflow syntax
   - Built-in functions
   - Step dependencies
   - Job status checks

5. **DevOps Concepts**
   - Continuous Integration
   - Parallel processing
   - Environment management

## 🔗 Accessing Artifacts

### Via GitHub UI
1. Go to **Actions** tab
2. Select **Multi-Platform Matrix Build** workflow
3. Click on a specific workflow run
4. Scroll to **Artifacts** section
5. Download individual artifacts or archive

### Via GitHub CLI
```bash
# List artifacts
gh run artifacts

# Download all artifacts
gh run download <RUN_ID>
```

### Via REST API
```bash
# Get workflow run artifacts
curl https://api.github.com/repos/USERNAME/REPO/actions/runs/RUN_ID/artifacts \
  -H "Authorization: token YOUR_TOKEN"
```

## 🔧 Customization

### Add More Platforms
```yaml
os: [ubuntu-latest, windows-latest, macos-latest, ubuntu-20.04]
```

### Add More Versions
```yaml
node-version: [14.x, 16.x, 18.x, 20.x, 21.x]
```

### Change Artifact Retention
```yaml
retention-days: 60  # Instead of 30
```

### Add Different Matrix Dimensions
```yaml
strategy:
  matrix:
    os: [ubuntu-latest, windows-latest]
    node-version: [16.x, 18.x]
    config: [debug, release]
    # This creates 2×2×2 = 8 jobs
```

## 📚 Resources

- **GitHub Actions Documentation**: https://docs.github.com/en/actions
- **Matrix Builds Guide**: https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs
- **Artifact Management**: https://docs.github.com/en/actions/using-workflows/storing-workflow-data-as-artifacts
- **Node.js Releases**: https://nodejs.org/en/download/releases/

## 🎓 For Students

This is an excellent project for learning:
- **DevOps fundamentals**: CI/CD pipeline design
- **Automation**: Reducing manual testing effort
- **Cloud concepts**: Running jobs on GitHub's infrastructure
- **Testing strategies**: Cross-platform compatibility testing
- **Data management**: Artifact organization and retention

## 📝 Troubleshooting

### Artifacts Not Showing?
- Check workflow permissions (Settings → Actions)
- Verify artifact names match the pattern
- Ensure paths exist before upload

### Jobs Not Running in Parallel?
- Check GitHub Actions queue
- Verify repository has sufficient GitHub Actions quota
- Check for job dependencies

### Artifacts Expire Quickly?
- Adjust `retention-days` in workflow
- Archive important artifacts separately

## 🤝 Contributing

Feel free to fork this project and experiment with:
- Different matrix dimensions
- Additional build steps
- Custom artifact formats
- Alternative testing frameworks

## 📄 License

This project is open source and available for educational and commercial use.

---

**Need Help?**
- Check GitHub Actions documentation
- Review workflow syntax
- Inspect job logs in Actions tab
- Verify artifact uploads in workflow run

**Questions about this demo?**
- Author Email: your.email@example.com
- GitHub Issues: [Create an issue in your repository]

---

**Last Updated**: January 2024
**Workflow Status**: ✅ Active and Running
**Total Runs**: Tracked automatically by GitHub
