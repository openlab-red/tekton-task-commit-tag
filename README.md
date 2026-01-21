# Tekton Task: commit-tag

A Tekton Task for committing changes and creating annotated git tags in release pipelines.

## Description

This task:
- Commits all staged changes with a customizable commit message
- Creates an annotated git tag for the release
- Pushes both the commit and tag to the remote repository
- Supports both HTTPS and SSH authentication

## Usage

### With Git Resolver

```yaml
taskRef:
  resolver: git
  params:
    - name: url
      value: https://github.com/openlab-red/tekton-task-commit-tag.git
    - name: revision
      value: main
    - name: pathInRepo
      value: task/commit-tag.yaml
```

### Example Pipeline Task

```yaml
- name: commit-and-tag
  params:
    - name: version
      value: "1.0.0"
    - name: tag-prefix
      value: "v"
    - name: branch
      value: "main"
    - name: git-user-name
      value: "tekton-pipeline"
    - name: git-user-email
      value: "tekton@pipeline.local"
  taskRef:
    resolver: git
    params:
      - name: url
        value: https://github.com/openlab-red/tekton-task-commit-tag.git
      - name: revision
        value: main
      - name: pathInRepo
        value: task/commit-tag.yaml
  workspaces:
    - name: source
      workspace: source
    - name: git-credentials
      workspace: git-credentials
```

## Parameters

| Name | Type | Default | Description |
|------|------|---------|-------------|
| `version` | string | (required) | Version string for the release (e.g., 1.0.0) |
| `tag-prefix` | string | `v` | Prefix for git tags (e.g., 'v' for v1.0.0) |
| `branch` | string | `main` | Git branch to push to |
| `git-user-name` | string | `tekton-pipeline` | Git user name for commits |
| `git-user-email` | string | `tekton@pipeline.local` | Git user email for commits |
| `commit-message` | string | `chore(release): ${TAG_NAME}\n\n[skip ci]` | Custom commit message template |
| `image` | string | `registry.access.redhat.com/ubi9/ubi-minimal:latest` | Image with git CLI |

## Results

| Name | Description |
|------|-------------|
| `tag` | The git tag that was created |
| `commit` | The commit SHA of the release commit |

## Workspaces

| Name | Optional | Description |
|------|----------|-------------|
| `source` | No | Workspace containing the git repository |
| `git-credentials` | Yes | Workspace containing git credentials |

### Git Credentials Workspace

The `git-credentials` workspace supports two authentication methods:

**HTTPS Authentication:**
- Place a `.git-credentials` file in the workspace
- Format: `https://username:token@github.com`

**SSH Authentication:**
- Place an `id_rsa` private key file in the workspace
- The task will automatically configure SSH and add known hosts

## License

Apache 2.0
