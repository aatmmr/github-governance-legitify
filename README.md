# GitHub Governance: Legitify Policy Scan

Repository to demonstrate use of [Legit-Labs Legitify](https://github.com/legit-labs/legitify) for repository governance.

## Policies

Legitify check for a number policies that are documented in [Legitify's docs](https://legitify.dev/github/).

## Workflows

Legitify provides a GitHub action that can be used. The action needs authentication through a [personal access token](#personal-access-token-pat). As this is not neccessarily best practice, an approach using a [GitHub App](#github-app) is investigated as well. Read the next section to get further details about the two methods.

### [GitHub App](./.github/workflows/legitify-repo-scan-gh-app.yml)

The target is to perform the scan through a GitHub app to avoid using a PAT. This does not work yet and needs to be further investigated.

### [Personal Access Token (PAT)](./.github/workflows/legitify-repo-scan-pat.yml)

This is workflow scans this repository manually according to Legitify's default policies. In a productive setting this scan should be scheduled on a daily or weekly basis.

The workflow creates an issue on success or failure to notify about the results. The issues are created from templates located at `.github/templates`. The templates contain placeholder that are replaced in the workflow run using [`jinja2`](https://jinja.palletsprojects.com/en/3.0.x/).

The required PAT need the following scopes: 

- `admin:org`
- `admin:org_hook`
- `read:enterprise`
- `read:org`
- `read:repo_hook`
- `repo`

The workflow expects a PAT called `GH_PAT` to be available as a repository secret. Please note that Fine-Grained PATs are not supported yet.

#### Scopes

Depending on the target of scanning, the scopes can vary. Please check the [requirements](https://github.com/legit-labs/legitify?tab=readme-ov-file#requirements).

For example, if only a repository is scanned, the following scopes are sufficient: `read:repo_hook, repo`

## Results

Results can be shown in a variety of ways. The most useful way ist to push them to the Security reports of the repsitory as Legitify supports the SAST format as output. Other possibilities are to create issues, job summaries or send the content to an external system using the job summaries output content.