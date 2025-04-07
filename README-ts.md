# Development Guidelines for Our HuggingFace Fork

This is a developer guide for working with our fork of the HuggingFace repository. We follow trunk-based development practices with release branching.

## Getting Started

### Clone the Repository

```bash
git clone https://github.com/OUR-ORGANIZATION/repository-name.git
cd repository-name
```

### Set Up Upstream Remote

```bash
git remote add upstream https://github.com/huggingface/repository-name.git
git fetch upstream
```

Verify your remotes:
```bash
git remote -v
```

## Development Workflow

We follow trunk-based development practices:

1. Most development happens directly on `main` or in short-lived feature branches
2. We sync frequently with the upstream HuggingFace repository
3. We use feature flags for in-progress work
4. We maintain release branches for stable versions

### Daily Development

#### For Small Changes (Preferred Method)

Work directly on the main branch:

```bash
git checkout main
git pull origin main
# Make your changes
git add .
git commit -m "Descriptive message about your changes"
git push origin main
```

#### For Larger Changes (1-2 Days Maximum)

Use a short-lived feature branch:

```bash
git checkout main
git pull origin main
git checkout -b feature-x
# Make your changes
git add .
git commit -m "Implement feature X"

# Stay up to date with main while working
git checkout main
git pull origin main
git checkout feature-x
git merge main

# When complete (within 1-2 days maximum)
git checkout main
git merge feature-x
git push origin main
git branch -d feature-x
```

### Using Feature Flags

For larger features that need to be merged before completion:

```python
# Example feature flag implementation
if ENABLE_MY_CUSTOM_FEATURE:
    # Your in-progress code here
```

Feature flags are defined in `config/feature_flags.py`.

### Syncing with Upstream HuggingFace Repository

Our team syncs with the upstream repository on Mondays and Thursdays:

```bash
git checkout main
git fetch upstream
git merge upstream/main
# Resolve any conflicts
git push origin main
```

## Release Management

We use release branches for stable versions:

### Creating a Release Branch

When ready to prepare a release:

```bash
git checkout main
git pull origin main
git checkout -b release/v1.0
git push origin release/v1.0
```

### Fixing Bugs in a Release Branch

Bug fixes should be applied to main first, then cherry-picked to the release branch:

```bash
# Fix the bug in main first
git checkout main
# Make and commit your fix
git push origin main

# Then apply to the release branch
git checkout release/v1.0
git cherry-pick <commit-hash>
git push origin release/v1.0
```

### Finalizing a Release

Once a release branch is stable:

```bash
git checkout release/v1.0
# Verify everything works
git tag v1.0.0
git push origin v1.0.0
```

## Contribution Guidelines

1. **Keep changes small**: Smaller commits are easier to review and less likely to cause conflicts
2. **Use descriptive commit messages**: Start with a verb and explain the purpose of the change
3. **Write tests**: All new features and bug fixes should include tests
4. **Document your changes**: Update the documentation when modifying functionality
5. **Review code**: All code should be reviewed before being merged to main

## Managing Our Customizations

Our custom code is organized as follows:

/custom_extensions/ - Our completely new modules
/patches/ - Documentation of changes to upstream files


After each upstream sync, run the test suite to verify functionality:

```bash
pytest
```

## Troubleshooting

### Resolving Merge Conflicts

When syncing with upstream, you may encounter merge conflicts:

```bash
# During merge, if conflicts happen:
# 1. Open the conflicted files and resolve the conflicts
# 2. Mark as resolved
git add <resolved-file>
# 3. Complete the merge
git commit
```

---

*Last synced with HuggingFace upstream on: [DATE]*  
*Current base HuggingFace version: [VERSION]*
*Active release branches: release/v1.0, release/v0.9*