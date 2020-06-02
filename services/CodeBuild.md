# AWS CodeBuild
CodeBuild is a fully managed continuous integration service that compiles source code, runs tests and produces software packages that are ready to deploy. With CodeBuild you don't need to provision, manage and scale your own build servers.

## Useful Links
- [AWS - CodeBuild FAQs](https://aws.amazon.com/codebuild/faqs)
- [YouTube - AWS re:Invent 2017: Continuous Integration Best Practices for Software Development Teams (DEV322)](https://www.youtube.com/watch?v=GEPJ7Lo346A)

## General Notes
- The build specification is defined on a `buildspec.yml` file
    - File should be on the root directory, otherwise you need to provide the path where it is stored
    - You can **override** the `buildspec.yml` file if necessary, using a different buildspec file or storing it in a **different directory or in a S3 bucket**
        - The S3 bucket needs to be **on the same region** as the build project
- You can view the full log in CloudWatch
- You can setup notifications
- Pricing:
    - Free tier includes 100 minutes of build.general1.small per month and does not expire after the 12-month free tier term
    - After that, princing depends on the size of build instance type and region
- The source code the following source code providers:
    - CodeCommit
    - Amazon S3 (provide bucket name and object name corresponding to the build input ZIP file with the source code)
    - GitHub
    - Bitbucket
- You can specify a linux user to run commands in the builspec file, using `run-as`
    - When you specify it at the top of the file, it applies globally
    - If you don't want to give permission to all commands, you can specify it on a specific phase
    - If `run-as` is not specified, then all commands runs as the root user

## Buildspec Reference
```yml
# required, recommended 0.2, but 0.1 is still supported
version: 0.2

# optional, available to linux users only. specifies linux user that runs commands in this buildspec file, granting the user read and execute permissions
run-as: linux-user-name

# optional
env:
    # not recommended for sensitive values
    variables:
        key: "value"
        key: "value"
    parameter-store:
        key: "value"
        key: "value"
    exported-variables:
        - variable
        - variable
    secrets-manager:
        key: secret-id:json-key:version-stage:version-id
    git-credential-helper: yes

# optional, used to represent settings if you run your build in an explicit proxy server
proxy:
    upload-artifacts: yes
    logs: yes    

# required
phases:
  install:
    run-as: Linux-user-name
    runtime-versions:
      runtime: version
      runtime: version
    commands:
      - command
      - command
    finally:
      - command
      - command
  pre_build:
    run-as: Linux-user-name
    commands:
      - command
      - command
    finally:
      - command
      - command
  build:
    run-as: Linux-user-name
    commands:
      - command
      - command
    finally:
      - command
      - command
  post_build:
    run-as: Linux-user-name
    commands:
      - command
      - command
    finally:
      - command
      - command
reports:
  report-name-or-arn:
    files:
      - location
      - location
    base-directory: location
    discard-paths: yes
    file-format: JunitXml | CucumberJson | VisualStudioTrx | TestNGXml
artifacts:
  files:
    - location
    - location
  name: artifact-name
  discard-paths: yes
  base-directory: location
  secondary-artifacts:
    artifactIdentifier:
      files:
        - location
        - location
      name: secondary-artifact-name
      discard-paths: yes
      base-directory: location
    artifactIdentifier:
      files:
        - location
        - location
      discard-paths: yes
      base-directory: location
cache:
  paths:
    - path
    - path
```