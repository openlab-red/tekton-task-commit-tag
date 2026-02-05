# Commit Tag Task

Tekton Task for committing changes and creating git tags.

## Usage

```yaml
taskRef:
  resolver: git
  params:
    - name: url
      value: https://azuredevops.alinma.internal/DevSecOps/tekton-tasks/_git/tekton-task-commit-tag
    - name: revision
      value: main
    - name: pathInRepo
      value: task/commit-tag.yaml
```

## Parameters

| Name | Default | Description |
|------|---------|-------------|
| `tag` | required | Tag to create |
| `message` | required | Commit/tag message |
| `push` | `true` | Push to remote |

## Workspaces

| Name | Description |
|------|-------------|
| `source` | Git repository |
| `git-credentials` | Credentials for push |

## Related Documentation

- [tekton-task-git-cliff](/docs/default/component/tekton-task-git-cliff/) - Auto versioning
