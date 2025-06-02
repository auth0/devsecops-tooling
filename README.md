# Devsecops-tooling
Contains tooling for scanning public sourcecode.




## Scan Option 1 - Reusable Workfow
If you do not have a build workflow/automation in GitHub CI then use this step. This option is intended to be a standalone integration and is implemented as its own job. 
```
name: SCA
on:
  push:
    branches: ["master", "main"]
jobs:
  snyk-cli:
    uses: auth0/devsecops-tooling/.github/workflows/sca-scan.yml@main
    with:
      additional-arguments: "--exclude=README.md,.jfrog"
    secrets: inherit
```

## Scan Option 2 - Reusable Action
If you already have a build workflow that installs dependencies. This option is intended to be used as a step within the build/test job immediately following the installation of dependencies.
```
      - name: Run Snyk
        uses: auth0/devsecops-tooling/.github/actions/sca-scan@main
        with:
          additional-arguments: ""
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}
          SIGNAL_HANDLER_TOKEN: ${{ secrets.SIGNAL_HANDLER_TOKEN }}
          SIGNAL_HANDLER_DOMAIN: ${{ secrets.SIGNAL_HANDLER_DOMAIN }}
```