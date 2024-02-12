# Octopus Deploy Github Action

The `Octopus Deploy` GitHub action is designed to create a release and deployment for Octopus Deploy. It is designed to 
be used with the following channels in octopus deploy: Pull Request, Development, and Release.

- **Pull Request**: This channel is used to create a package and release for pull requests. It will be used when the action is
triggered by a pull request.
- **Development**: This channel is intended to be used for continuous deployment to a development environment. It will be
used when a push is made to the default branch.
- **Release**: This channel will create a package and release for a release branch or tag. This is the default channel
if the event is not a pull request or a push to the default branch.


## Inputs

- `octopus_server`: The URL of your Octopus Server. This is required.
- `octopus_api_key`: Your Octopus API Key. This is required.
- `octopus_space`: The Octopus Space to use. This is required.
- `package_id`: The Octopus Package ID. This is required.
- `project_id`: The Octopus Project ID. This is required.
- `version`: The version of the package. This is required.
- `path`: The path to the package contents. This defaults to './out'.
- `alt_packages`: Additional packages to include in the octopus release. Not required.
- `jira_id`: The Jira story ID (e.g. PROD-1234) used for release notes. Not required.
- `variables`: Octopus Release Variables to include. Not required. 
- `release`: Whether to create a release (in addition to a package) in Octopus Deploy. Defaults to 'true'.
- `tenant`: The Octopus Tenant to deploy to. Defaults to 'Current'.
- `environment`: The Octopus Environment to deploy to. This defaults to 'Development' and is only used when creating a deployment from the default branch.

## Outputs

- `version`: The version of the image. The value is derived from the `release_number` output of the `release` step.

## Process

The action will perform the following steps:
<!--  This action will create a zip package, push the package and build information to Octopus, create a release in Octopus Deploy, deploy the release, and await the task in Octopus Deploy. If the event is a pull request, it will also add a comment with the release information. -->

1. **Create Package**: The action will create a package in Octopus Deploy. 
2. **Push Package**: The action will push the package and build information to Octopus Deploy.
3. **Create Release**: The action will create a release in Octopus Deploy. If the event is a pull request, it will
create a release in the Pull Request channel. If the event is a push to the default branch, it will create a release
in the Development channel. If the event is a release branch or tag, it will create a release in the Release channel.
4. **Deploy Release**: If the event is a push to the default branch, the action will deploy the release to the specified environment.
5. **Await Task**: If the event is a push to the default branch, the action will wait for the deployment task to complete.
6. **Add Comment**: If the event is a pull request, the action will add a comment with the release information.

## Usage

To use this action in your workflow, include it as a step in your workflow file. Here is an example:

```yaml
- name: Octopus Deploy
  uses: enosix/ghac-octopus@stable
  with:
    octopus_server: 'https://your-octopus-server.com'
    octopus_api_key: ${{ secrets.OCTOPUS_API_KEY }}
    octopus_space: 'your-space'
    package_id: 'your-package-id'
    project_id: 'your-project-id'
    version: '1.0.0'
    path: './out'
    alt_packages: |
      package1:1.0.0
      package2:1.0.0
    jira_id: 'PROD-1234'
    variables: |
      Variable1=Value1
      Variable2=Value2
    tenant: 'Current'
    environment: 'Development'
```

Replace the `octopus_server`, `octopus_api_key`, `octopus_space`, `package_id`, `project_id`, `version`, `path`, `alt_packages`, `jira_id`, `variables`, `tenant`, and `environment` with your own values.
Make sure to include the `octopus_api_key` in your repository secrets.
