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



## RL scan Option 1 - Reusable Workflow
This option is intended to be a standalone integration and is implemented as its own job. It requires that the artifact to be scanned is available at the specified path.
```
name: RL Scan
on:
  push:
    branches: ["master", "main"]
jobs:
  rl-scan:
    uses: auth0/devsecops-tooling/.github/workflows/rl-scanner.yml@main
    with:
      artifact-path: "path/to/your/artifact"
      version: "version of your artifact"
    secrets: inherit
```

### Outputs
| Output | Description |
|--------|-------------|
| `scan-status` | The outcome of the scan process (`success` or `failed`). |

## RL scan Option 2 - Reusable Action
If you already have a build workflow that produces an artifact, this option is intended to be used as a step within the build job immediately following the artifact creation.
```
      - name: Run RL Scanner
        uses: auth0/devsecops-tooling/.github/actions/rl-scan@main
        with:
          artifact-path: "path/to/your/artifact"
          version: "version of your artifact"
          RLSECURE_LICENSE: ${{ secrets.RLSECURE_LICENSE }}
          RLSECURE_SITE_KEY: ${{ secrets.RLSECURE_SITE_KEY }}
          SIGNAL_HANDLER_TOKEN: ${{ secrets.SIGNAL_HANDLER_TOKEN }}
          PRODSEC_TOOLS_ARN: ${{ secrets.PRODSEC_TOOLS_ARN }}
          PRODSEC_TOOLS_USER: ${{ secrets.PRODSEC_TOOLS_USER }}
          PRODSEC_TOOLS_TOKEN: ${{ secrets.PRODSEC_TOOLS_TOKEN }}
          PRODSEC_PYTHON_TOOLS_REPO: ${{ secrets.PRODSEC_PYTHON_TOOLS_REPO }}
```


### Outputs
| Output | Description |
|--------|-------------|
| `scan-status` | The outcome of the scan process (`success` or `failed`). |