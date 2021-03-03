# Change Log

All notable changes to this project will be documented in this file. The format is based on [Keep a Changelog](http://keepachangelog.com/).
> Sections: (`Added`, `Changed`, `Deprecated`, `Removed`, `Fixed`, `Security`, `API Changes`, `Entity Changes`)

## [0.6.0] - 2020-10-26

### Added
- Added command: `hal/login-to-ecr`
  > This command is used to sign into AWS ECR with `docker login` using credentials for Hal for AWS accounts connected
  > to an existing Hal organization.
  >
  > Usage example:
  >
  > ```
  > orbs:
  >     hal: quickenloans/hal@x.y.z
  > jobs:
  >     my_example_job:
  >         environment:
  >             HAL_ORGANIZATION: '0000'
  >             ECR_REGION: 'us-east-1'
  >         steps:
  >             - setup_remote_docker:
  >                 docker_layer_caching: true
  >
  >              - hal/login-to-ecr:
  >                  organization: '$HAL_ORGANIZATION'
  >                  environment: 'test'
  >                  region: '$ECR_REGION'
  > ```


## [0.5.0] - 2020-06-08

- Added `api-token-var` parameter to the `hal/build`, `hal/publish`, and `hal/deploy` jobs.
  > This parameter can be used to customize the environment variable used for the api token.

## [0.4.0] - 2020-06-05

- Added `artifact-file` parameter to the `hal/publish` job.
  > Use `artifact-file` instead of `artifact-path` if you build your artfact zip or tgz through other tooling.

## [0.3.0] - 2020-06-04

- Added `skip-job-var` parameters to the `hal/build`, `hal/publish`, and `hal/deploy` jobs.
  > Environment variable that jobs in this Orb detect. If set to `1` they will skip (silently with exit code 0) their
  > steps.
- Added `before-steps` parameters to the `hal/build`, `hal/publish`, and `hal/deploy` jobs.
  > Run any CircleCI steps after workspace attachment, but before the Hal API is called. Typically used to determine
  > if Hal steps should be skipped (by setting the environment variable `skip-job-var`.

## [0.2.1] - 2020-06-04

- Added `arguments` parameters to the `hal/build`, `hal/publish`, and `hal/deploy` jobs.
  > Used to pass arbitrary extra arguments to the hal scripts. Should be used for job parameters (metadata).

## [0.2.0] - 2020-06-04

### Added
- Added `environment` parameter to the `hal/build` and `hal/publish` jobs.
  > This parameter can be used to specify an environment ID or name. The default behavior
  > is to create a build compatible with any environment. When this is used the build can only be deployed
  > to this environment.
- Added job: `hal/deploy`
  > This is job can trigger a deployment to Hal. It is compatible with the `hal/publish` and `hal/build` steps
  > to deploy a build to an environment (deployment setting). It can also be used to deploy to a target that
  > does not require a build.
  >
  > Usage example:
  >
  > ```
  > orbs:
  >     hal: quickenloans/hal@x.y.z
  > workflows:
  >     my_example_pipeline:
  >         jobs:
  >             - hal/deploy:
  >                 hal-appid: 'XXXXXX'
  >                 deploy-setting-id: 'YYYYYY'
  >                 wait-for-running-jobs: true # Default value (can be deleted)
  >                 workspace-root: '.'         # Default value (can be deleted)
  >                 job-file: '.hal_deploy_id'  # Default value (can be deleted)
  >                 arguments: '--meta-myvar1=example1 --meta-myvar2=example2'
  > ```

## [0.1.0] - 2020-06-03

### Added
- Added job: `hal/checkout`
  > This is a simple job that is only used to check out code and persist those files.
  > Projects should typically checkout only once, and pass those files in workspaces to further jobs in the pipeline.
  >
  > Usage example:
  >
  > ```
  > orbs:
  >     hal: quickenloans/hal@x.y.z
  > workflows:
  >     my_example_pipeline:
  >         jobs:
  >             - hal/checkout:
  >                 workspace-root: '.' # Default value (can be deleted)
  >                 persist-path: '.'   # Default value (can be deleted)
  > ```
- Added job: `hal/publish`
  > This job is used to publish a build and transfer an artifact from CircleCI to Hal. Do not use
  > if no artifact is passed to Hal (Use `hal/build`).
  >
  > Usage example:
  >
  > ```
  > orbs:
  >     hal: quickenloans/hal@x.y.z
  > workflows:
  >     my_example_pipeline:
  >         jobs:
  >             - hal/publish:
  >                 requires: [ hal/checkout ]
  >                 context: 'my_secrets'
  >
  >                 hal-appid: '000000'
  >                 workspace-root: '.'        # Default value (can be deleted)
  >                 job-file: '.hal_build_id'  # Default value (can be deleted)
  >                 artifact-path: './mybuildfiles'
  >                 artifact-compression: 'tgz'
  > ```
- Added job: `hal/build`
  > This job is used to start a build. Do not use if an artifact is need to be passed to Hal (Use `hal/publish`).
  >
  > Usage example:
  >
  > ```
  > orbs:
  >     hal: quickenloans/hal@x.y.z
  > workflows:
  >     my_example_pipeline:
  >         jobs:
  >             - hal/publish:
  >                 requires: [ hal/checkout ]
  >                 context: 'my_secrets'
  >
  >                 hal-appid: '000000'
  >                 workspace-root: '.'        # Default value (can be deleted)
  >                 job-file: '.hal_build_id'  # Default value (can be deleted)
  >                 artifact-path: './mybuildfiles'
  >                 artifact-compression: 'tgz'
  > ```
- Added command: `hal/install-certificates`
  > This command is used to install company certificates for access to APIs or Applications that use an Internal CA for SSL.
  >
  > Usage example:
  >
  > ```
  > orbs:
  >     hal: quickenloans/hal@x.y.z
  > jobs:
  >     my_example_job:
  >         steps:
  >             - hal/install-certificiates
  > ```
