# ready-for-review-pr
GitHub Action to Take a PR from Draft to Ready-for-Review

Converts the current (or specified) pull request tobe ready-to-review (no longer draft)

## Inputs

| Name                     | Type   | Required?                            | Description                                                                                                                                                                    |
|--------------------------|:------:|:------------------------------------ |:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `token`| string | false (defaults to GITHUB_TOKEN) |  The GitHub token to allow authenticated actions |
| `pull-request-number`| string | false (defaults to workflow pr in pull_request event) | The GitHub node id of the pull request to mark as ready-to-review |
| 'owner' | string | false (defaults to current repo's owner) | the owner or organization where the repo exists |
| 'repo' | string | false (defaults to current repo) | the repo name where the PR exists |

## Usage

```yaml
# Marks all newly opened pull requests as ready-to-review
name: Mark as Ready to Review on Open
on:
  pull_request:
    types: [ opened ]

jobs:
  mark-as-ready-to-review:
    name: Mark as Ready to Review
    runs-on: ubuntu-latest
    steps:
      - name: Mark as Ready to Review
        uses: leemeador/ready-for-review-pr@v1.0.4 # please use latest version
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          pull-request-number: 77
          owner: leemeador
          repo: repo-name
```

### Retrieving the PR number

You can see the PR number in the browser address bar when you view a PR on github.com. But this doesn't help much if you need to use it in a workflow.

When calling this action from a workflow triggered by a `pull_request` event, it will automatically apply to the current pull request. 

If you want to apply this action to a different PR, you can retrieve its number by calling the command `gh pr view $PR --json number --jq .number` where `$PR` is the number, url, or branch of your PR.

For a 'pull-request' event you can get the number this way (or just let it default):

  - run: |
      echo github.event.number = ${{ github.event.number  }}
      echo github.event.pull_request.number = ${{ github.event.pull_request.number }}
      echo github.event.issue.number = ${{ github.event.issue.number }}

This GitHub Marketplace action says it will find the PR number for a push event:

  https://github.com/marketplace/actions/find-current-pull-request
  
## License

MIT
