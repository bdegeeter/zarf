# Contributing to Zarf

First off, thanks so much for wanting to help out! :tada:

This document describes the steps and requirements for contributing a bug fix or feature in a Pull Request to Zarf!  If you have any questions about the process or the pull request you are working on feel free to reach out in the [Zarf Dev Kubernetes Slack Channel](https://kubernetes.slack.com/archives/C03BP9Z3CMA). The doc also details a bit about the governance structure of the project.

## Developer Experience

Continuous Delivery is core to our development philosophy. Check out [https://minimumcd.org](https://minimumcd.org/) for a good baseline agreement on what that means.

Specifically:

- We do trunk-based development (`main`) with short-lived feature branches that originate from the trunk, get merged into the trunk, and are deleted after the merge
- We don't merge code into `main` that isn't releasable
- We perform automated testing on all changes before they get merged to `main`
- We create immutable release artifacts

### Pre-Commit Hooks and Linting

We use [pre-commit](https://pre-commit.com/) to manage our pre-commit hooks. This ensures that all code is linted and formatted before it is committed. After `pre-commit` is [installed](https://pre-commit.com/#installation):

```bash
# install hooks
pre-commit install

# install golang-ci-lint
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```

Now every time you commit, the hooks will run and format your code, linting can be called via `make lint-go`.

### Contributing Guidelines 

Zarf is a tool used within the United States Governement and as such security is our highest priority. Contributors external to Defense Unicorns or non-Zarf maintainers will require two (2) reviewers.

### Developer Workflow

:key: == Required by automation

1. Look at the next due [release milestone](https://github.com/zarf-dev/zarf/milestones) and pick an issue that you want to work on. If you don't see anything that interests you, create an issue and assign it to yourself.
1. Drop a comment in the issue to let everyone know you're working on it and submit a Draft PR (step 4) as soon as you are able. If you have any questions as you work through the code, reach out in the [Zarf Dev Kubernetes Slack Channel](https://kubernetes.slack.com/archives/C03BP9Z3CMA).
1. :key: Set up your Git config to GPG sign all commits. [Here's some documentation on how to set it up](https://docs.github.com/en/authentication/managing-commit-signature-verification/signing-commits). You won't be able to merge your PR if you have any unverified commits.
1. Create a Draft Pull Request as soon as you can, even if it is just 5 minutes after you started working on it. We lean towards working in the open as much as we can. If you're not sure what to put in the PR description, just put a link to the issue you're working on.

   - :key: We follow the [conventional commits spec](https://www.conventionalcommits.org/en/v1.0.0/) with the [commitlint conventional config](https://github.com/conventional-changelog/commitlint/tree/master/%40commitlint/config-conventional) as extended types for PR titles.

1. :key: Automated tests will begin based on the paths you have edited in your Pull Request.
   > ⚠️ **NOTE:** _If you are an external third-party contributor, the pipelines won't run until a [CODEOWNER](https://github.com/zarf-dev/zarf/blob/main/CODEOWNERS) approves the pipeline run._
1. :key: Be sure to use the [needs-adr,needs-docs,needs-tests](https://github.com/zarf-dev/zarf/labels?q=needs) labels as appropriate for the PR. Once you have addressed all of the needs, remove the label.
1. Once the review is complete and approved, a core member of the zarf project will merge your PR. If you are an external third-party contributor, two core members of the zarf project will be required to approve the PR.
1. Close the issue if it is fully resolved by your PR. _Hint: You can add "Fixes #XX" to the PR description to automatically close an issue when the PR is merged._

## Testing

> A more comprehensive guide to testing can be found [here](https://docs.zarf.dev/contribute/testing).

Our E2E tests can be found in the `src/test` folder and follow the journey of someone as they would use the Zarf CLI. In CI these tests run against our currently supported cluster distros and are the primary way that Zarf code is tested.

Our unit tests can be found as `*_test.go` files inside the package that they are designed to test. These are also run in CI and are designed to test small functions with clear interfaces that would be difficult to test otherwise.

## Documentation

The CLI docs (located at `site/src/content/docs/commands`), and [`zarf.schema.json`](https://github.com/zarf-dev/zarf/blob/main/zarf.schema.json) are autogenerated from `make docs-and-schema`. Run this make target locally to regenerate the schema and documentation each time you make a change to the CLI commands or the schema, otherwise CI will fail.

We do this so that there is a git commit signature from a person on the commit for better traceability, rather than a non-person entity (e.g. GitHub CI token).

## Architecture Decision Records (ADR)

We've chosen to use ADRs to document architecturally significant decisions. We primarily use the guidance found in [this article by Michael Nygard](http://thinkrelevance.com/blog/2011/11/15/documenting-architecture-decisions) with a couple of tweaks:

- The criteria for when an ADR is needed is undefined. The team will decide when the team needs an ADR.
- We will use the tool [adr-tools](https://github.com/npryce/adr-tools) to make it easier on us to create and maintain ADRs.
- We will keep ADRs in the repository under `adr/NNNN-name-of-adr.md`. `adr-tools` is configured with a dotfile to automatically use this directory and format.

### How to use `adr-tools`

```bash
# Create a new ADR titled "Use Bisquick for all waffle making"
adr new Use Bisquick for all waffle making

# Create a new ADR that supersedes a previous one. Let's say, for example, that the previous ADR about Bisquick was ADR number 9.
adr new -s 9 Use scratch ingredients for all waffle making

# Create a new ADR that amends a previous one. Let's say the previous one was ADR number 15
adr new -l "15:Amends:Amended by" Use store-bought butter for all waffle making

# Get full help docs. There are all sorts of other helpful commands that help manage the decision log.
adr help
```

## Governance

### Technical Steering Committee
The Technical Steering Committee (the “TSC”) will be responsible for all technical oversight of the project. The TSC may elect a TSC Chair, who will preside over meetings of the TSC and will serve until their resignation or replacement by the TSC. Current members of the TSC include:

#### Austin Abro
Affiliation: Defense Unicorns
GitHub: @AustinAbro321

#### Danny Gershman
Affiliation: Radius Method
GitHub: @dgershman

#### Jeff McCoy (TSC Chair)
Affiliation: Defense Unicorns
GitHub: @jeff-mccoy

#### Kit Patella
Affiliation: Defense Unicorns
GitHub: @mkcp

#### Wayne Starr
Affiliation: Defense Unicorns
GitHub: @Racer159

