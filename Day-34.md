# Automatically Create Release Tags on GitHub Push

This guide explains how to automatically create a date-based Git tag whenever code is pushed to the `master` branch of a GitHub repository.

> **Recommended approach:** Use GitHub Actions. GitHub does not provide access to its server-side `hooks/post-update` directory, so you cannot install a traditional server-side Git hook on GitHub-hosted repositories.

## 1. How it works

1. A developer pushes a commit to `master`.
2. GitHub detects the push and starts the workflow.
3. The workflow creates a tag using the current date, for example `release-2026-11-22`.
4. The workflow pushes that tag back to the repository.

## 2. Create the workflow file

In your local repository, create this directory and file:

```text
.github/
└── workflows/
    └── release-tag.yml
```

For example:

```bash
mkdir -p .github/workflows
vi .github/workflows/release-tag.yml
```

## 3. Add the GitHub Actions workflow

Add the following YAML:

```yaml
name: Create Release Tag

on:
  push:
    branches:
      - master

permissions:
  contents: write

jobs:
  create-tag:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          fetch-depth: 0

      - name: Create and push release tag
        env:
          GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        run: |
          TAG="release-$(date -u +%Y-%m-%d)"

          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"

          if git ls-remote --exit-code --tags origin "refs/tags/$TAG" >/dev/null 2>&1; then
            echo "Tag $TAG already exists. Skipping tag creation."
            exit 0
          fi

          git tag "$TAG" "$GITHUB_SHA"
          git push origin "refs/tags/$TAG"
```

### Explanation

| Configuration | Purpose |
|---|---|
| `on.push.branches: master` | Runs the workflow when a commit is pushed to `master`. |
| `permissions: contents: write` | Allows the workflow token to create and push tags. |
| `actions/checkout` | Checks out the repository. |
| `date -u +%Y-%m-%d` | Generates a UTC date in `YYYY-MM-DD` format. |
| `GITHUB_SHA` | Identifies the commit that triggered the workflow. |
| `git tag` | Creates the release tag. |
| `git push origin` | Pushes the tag to GitHub. |

The workflow uses the automatically provided `GITHUB_TOKEN`; you do not need to create a personal access token for this example.

The example deliberately skips creation if a tag with that date already exists. This avoids silently moving an existing release tag when there are multiple pushes on the same day.

## 4. Commit and push the workflow

```bash
git add .github/workflows/release-tag.yml
git commit -m "Add automatic release tagging workflow"
git push origin master
```

The push adds the workflow to GitHub. It should also trigger the workflow because it updates `master`.

## 5. Check the workflow run on GitHub

1. Open your GitHub repository.
2. Select the **Actions** tab.
3. Open **Create Release Tag**.
4. Select the latest workflow run.
5. Check that each step completed successfully.

If the workflow fails, open the failed step to read its error message.

## 6. Verify the release tag

On GitHub:

1. Open the repository.
2. Select **Releases** or the **Tags** view (often available from the repository's Code page).
3. Look for a tag such as `release-2026-11-22`.

You can also verify from your local clone:

```bash
git fetch --tags
git tag -l "release-*"
git show release-2026-11-22
```

Replace the example date with the date of the tag you are checking.

## 7. Test with another push

Make a small change and push it to `master`:

```bash
echo "Testing automatic release tagging" >> test-hook.txt
git add test-hook.txt
git commit -m "Test automatic release tagging"
git push origin master
```

Then check **GitHub → Actions** and fetch the tags:

```bash
git fetch --tags
git tag -l "release-*"
```

If a tag for that date already exists, the workflow intentionally skips creating another one. To test the workflow again on the same date, inspect the existing tag and workflow logs; do not delete or move a release tag unless you are sure that is safe for your repository.

## 8. Important notes

- **GitHub-hosted repository:** Use GitHub Actions rather than trying to edit GitHub's server-side `hooks/post-update` directory.
- **Branch name:** If your default branch is `main`, change `master` to `main` in the workflow.
- **Date and timezone:** This example uses UTC (`date -u`). If releases must use a particular local timezone, change the date command accordingly, for example `TZ=Asia/Kolkata date +%Y-%m-%d`.
- **One tag per day:** The example creates at most one `release-YYYY-MM-DD` tag per day. If you need multiple releases on the same day, use a version number or timestamp in the tag name.
- **Tag protection:** Repository rulesets or tag protection rules may prevent the workflow from creating tags. Check repository rules if the push is rejected.
- **Workflow permissions:** The repository's Actions settings must allow the `GITHUB_TOKEN` to have read and write permissions. The workflow's `contents: write` permission requests the required repository-content permission, subject to organization and repository policy.
- **Trusted code:** Only use workflows and third-party actions you trust. For production, consider pinning third-party actions to reviewed commit SHAs.

## 9. Git hooks vs. GitHub Actions

| Feature | Server-side Git hook | GitHub Actions |
|---|---|---|
| Where it runs | On a Git server you administer | GitHub-hosted runner |
| Can edit `hooks/post-update` on GitHub.com? | No | Not applicable |
| Trigger | Git server receives a push | Configured GitHub event, such as a push to `master` |
| Typical use | Self-hosted Git repositories | GitHub CI/CD automation |

## Final checklist

- [ ] Created `.github/workflows/release-tag.yml`.
- [ ] Configured the workflow to trigger on pushes to `master`.
- [ ] Granted `contents: write` permission.
- [ ] Committed and pushed the workflow file.
- [ ] Confirmed the workflow succeeded in the Actions tab.
- [ ] Confirmed the date-based tag exists in GitHub.
- [ ] Tested the behavior with a subsequent push.

**Expected tag format:** `release-YYYY-MM-DD`
