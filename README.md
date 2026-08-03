# p6m7g8-actions/next-upgrade

- [p6m7g8-actions/next-upgrade](#p6m7g8-actionsnext-upgrade)
  - [Usage](#usage)
  - [Inputs](#inputs)

## Usage

```yaml
      - name: Upgrade Deps
        uses: p6m7g8-actions/p6-next-upgrade@main
        with:
          gh_token: ${{ secrets.P6_PGOLLUCCI_GH_TOKEN }}
```

## Inputs

| Input | Required | Default | Description |
| --- | --- | --- | --- |
| `gh_token` | yes | n/a | Automation token (`P6_A_GH_TOKEN`). Re-labels the PR and reopens it so checks dispatch. |
| `git_token` | no | `""` | Optional extra fallback token for branch operations. |
| `hold_dependencies` | no | `typescript` | Packages whose `package.json` range must survive `pnpm up --latest`. |

`hold_dependencies` accepts spaces, newlines, or both, so either form works:

```yaml
        with:
          hold_dependencies: "typescript eslint"
```

```yaml
        with:
          hold_dependencies: |
            typescript
            eslint
```

The declared range is snapshotted before the upgrade and written back after,
then the lockfile is re-resolved. `dependencies`, `devDependencies` and
`optionalDependencies` are all searched, and a held package is written back to
the section it was declared in. A package this repo does not declare is
reported as a notice and skipped. Passing an empty value holds nothing, which
is not the same as passing nothing: the `typescript` default applies only when
the input is absent.

`typescript` is held by default because TS 7 breaks `build` fleet-wide; see the
Update Dependencies step in `action.yml` for the detail and for when to drop it.
