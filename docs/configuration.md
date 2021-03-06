---
layout: page
title: deploy.yml
---

# Configuration (`.github/deploy.yml`)

Deliverybot controls deployments based on a configuration file. Targets are the
top level resource for this file which house configuration to trigger a list of
deployments depending on specific conditions. Documentation for those conditions
and fields is provided below.

```yaml {% raw %}
# .github/deploy.yml
production:
  environment: name {% endraw %}
```

- [`<target>.auto_deploy_on`](#targetauto_deploy_on)
- [`<target>.transient_environment`](#targettransient_environment)
- [`<target>.production_environment`](#targetproduction_environment)
- [`<target>.required_contexts`](#targetrequired_contexts)
- [`<target>.environment`](#targetenvironment)
- [`<target>.task`](#targettask)
- [`<target>.auto_merge`](#targetauto_merge)
- [`<target>.payload`](#targetpayload)

##### `<target>.auto_deploy_on`

Controls auto deployment behaviour given a ref. If any new push events are
detected on this event, the deployment will be triggered.

```yaml
# .github/deploy.yml
production:
  auto_deploy_on: refs/heads/master
```

__Recently added!__:

This configuration option also supports a `*` character to match any subsequent
characters after it. This allows us to deploy on any tag (`refs/tags/*`) or to
match any branch for deployments (`refs/heads/*`).

Deploying on any tag is probably one of the most commonly used features for
deliverybot:

```yaml
  auto_deploy_on: refs/tags/*
```

##### `<target>.transient_environment`

Specifies if the given environment is specific to the deployment and will no
longer exist at some point in the future. Default: false

If this value is `true` all deployments triggered on a pr will be marked
inactive.

##### `<target>.production_environment`

Specifies if the given environment is one that end-users directly interact with.
Default: true when environment is production and false otherwise.

##### `<target>.required_contexts`

The status contexts to verify against commit status checks. Default: []

##### `<target>.environment`

The name of the environment that was deployed to (e.g., staging or production).
Default: none


##### `<target>.task`

The name of the task for the deployment (e.g., deploy or deploy:migrations).
Default: none

##### `<target>.auto_merge`

Attempts to automatically merge the default branch into the requested ref, if
it's behind the default branch. Default: false

##### `<target>.payload`

Payload with extra information about the deployment. Default: {}

## Variables

The following variables are available using `{% raw %}${{ }}{% endraw %}` syntax
when evaluating deployment targets:

- `ref`: Deployment ref, ie: `master`.
- `sha`: Full git sha.
- `short_sha`: Short git sha.
- `pr`: Pr number, if this deployment is kicked off in a PR.
- `target`: Current target name.
- `owner`: Owner name.
- `repo`: Repo name.
- `pull_request`: [GitHub pull request object.][pr]
- `commit`: [GitHub commit object.][commit]

An example usage of this:

```yaml {% raw %}
# .github/deploy.yml
review:
  # Dynamic environment name. The environment will look like pr123.
  environment: pr${{ pr }} {% endraw %}
```

[pr]: https://developer.github.com/v3/pulls/#response-1
[commit]: https://developer.github.com/v3/git/commits/#response
