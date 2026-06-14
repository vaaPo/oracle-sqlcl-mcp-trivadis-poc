# Git Workflow For Trivadis Iterations

## Goal

Use Git branches, commits, pull requests, and reviews to track Trivadis findings and fixes in repository source files.

## Suggested Workflow

1. Create a feature branch.
2. Add or update a SQL file, for example `testme.sql`.
3. Commit and push the initial source file.
4. Run SQLcl MCP Trivadis checks against the file.
5. Commit and push the findings and suggested fixes documentation.
6. Ask which warnings should be fixed now.
7. Fix the selected warnings in repository source files.
8. Commit and push the fixes.
9. Create a pull request for the branch.
10. Review the PR in GitHub.
11. Merge the branch to `main`.
12. Update `trivadis_warning_status.md` with the branch name and merge timestamp.

## Example Command Flow

Create branch:

```text
git switch -c feature/trivadis-testme
```

Create source file and commit it:

```text
git add testme.sql
git commit -m "Add test SQL for Trivadis review"
git push -u origin feature/trivadis-testme
```

Commit findings and suggested fixes:

```text
git add testme.sql trivadis_warning_status.md
git commit -m "Record Trivadis findings for testme"
git push
```

Commit selected fixes:

```text
git add testme.sql trivadis_warning_status.md
git commit -m "Fix selected Trivadis warnings in testme"
git push
```

Create PR:

```text
gh pr create --fill
```

Review PR and merge:

```text
gh pr view --web
gh pr merge --merge
```

## Example Prompts

Prompt for findings pass:

```text
Create a new branch for this SQL file, run SQLcl MCP Trivadis checks on the repository source file, record the findings and suggested source-file fixes, commit the findings, and push the branch.
```

Prompt for fix pass:

```text
Using the existing branch, fix only the selected Trivadis warnings in the repository source file, update trivadis_warning_status.md, commit the fixes, push the branch, and prepare the branch for PR review.
```

## Tracking Rule

- Keep one status row per source file and Trivadis rule.
- Use the same row across branch work, fixes, and merge.
- Fill `Branch Name` when work starts on the branch.
- Fill `Merged To Main Stamp` when the branch is merged to `main`.
