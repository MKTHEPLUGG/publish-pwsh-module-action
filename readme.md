# Publish PowerShell Module GitHub Action

A reusable GitHub Action to automate the build, tagging, validation, and publication of a PowerShell module to the PowerShell Gallery.

## Overview

This action is designed to streamline the process of publishing PowerShell modules by:
1. Checking out the code.
2. Tagging the module version with `GitVersion`.
3. Creating a module manifest.
4. Testing the manifest.
5. Publishing the module to the PowerShell Gallery.

## Usage

### Example Workflow

Below is an example of how to use this action in a GitHub workflow. This workflow will trigger on any `push` or `pull_request` event to the `main` branch when there are changes in the `pwsh/module/**` directory.

```yaml
name: Publish PowerShell Module

on:
  push:
    branches:
      - main
    paths:
      - 'pwsh/module/**'
  pull_request:
    branches:
      - main
    paths:
      - 'pwsh/module/**'

jobs:
  publish:
    runs-on: windows-latest

    steps:
      - name: Publish PowerShell Module
        uses: ./.github/actions/publish-pwsh-module
        with:
          psd1Path: "${{ github.workspace }}\\pwsh\\module\\PDS.psd1"
          psm1Path: "${{ github.workspace }}\\pwsh\\module\\PDS.psm1"
          companyName: "meti.pro"
          apiKey: ${{ secrets.API_KEY }}
          rootModule: "PDS"
          description: "Personal Deploy Script"
```

### Inputs

- `psd1Path` (required): The path to the PSD1 manifest file for the module.
- `psm1Path` (required): The path to the PSM1 module file.
- `companyName` (required): The company name as required by NuGet.
- `apiKey` (required, secret): NuGet API key to authenticate with the PowerShell Gallery.
- `rootModule` (required): The name of the root module.
- `description` (required): A description of the module.

### Prerequisites

- **PowerShellGet**: Ensure you have the latest version of PowerShellGet installed.
- **GitVersion Configuration**: The `gitversion.yml` file should be in the root directory or specified as needed.

### Steps in the Action

1. **Checkout Code**: Clones the repository with the full commit history.
2. **Tag Version**: Uses the `GitVersion` action to assign a version tag based on your Git history.
3. **Get Commit Message**: Retrieves the latest commit message for use in the release notes.
4. **Create Module Manifest**: Generates a new module manifest (`PSD1` file) with dynamic data.
5. **Test Module Manifest**: Validates the manifest file to ensure it’s ready for publication.
6. **Publish Module**: Uploads the PowerShell module to the PowerShell Gallery using the specified API key.

---