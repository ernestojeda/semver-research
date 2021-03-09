# Semver Option Impact Analysis

The question to answer is do we only want to focus on generating semantic versioning or do we want to find a tool that handles both semantic versioning + automated release management

## Semver + Release
* [semantic-release](#semantic-release)
* [Go Releaser](#go-releaser)
* [Go Semantic Release](#go-semantic-release)
* [Release It](#release-it)

## Semver Only

* [Standard Version](#standard-version)

## Not Considering

* [Release Please](#release-please)

## Semver + Release Approach

* Can do both semantic versioning and automated release
* Release process would probably move inline with each project
* Semver version is automatically calculated based on conventional commits
* If we allow go with the full release approach, we would probably need to scrap or rewrite most of what we have created in cd-management@release. There are pro's and cons to this decision
   * Pros:
     * Could potentially simplify release process, less yaml to manage
     * Using popular industry tool may allow others to step in and manage. Less reliance on custom tooling
     * If using plugins, most plugin ecosystem is rich and we _may_ be able to leverage existing tools to do most things out of the box
   * Cons:
     * Fully reliant on conventional commits for semver and release.
     * If we need custom functionality we would need to develop/maintain custom plugins/forks which may be written in other programming languages
     * Throw away two years worth of custom code
     * Small learning curve for semantic-release

### Potential new release flow (edgeXBuildGoApp)

#### Ideas for incorporating new change

1. Special new commit message to handle release in each specific repo.
2. PR is open and build runs in dry run mode

New Stages

`Semantic Release Prep (Generate Version)` > `Test` > `Build` > `Docker Push (Nexus)` > `CodeCov` > `Snyk Deps` > `Git Tag` > `Sigul Sign` > `Semantic Release Final Steps`

#### Step Summary

* Semantic Release Prep (Generate Version)
    * Analyze commits and generate semver version
    * Semver version is extracted and stored in `VERSION` env var
* Test, no change
* Build, no change
* Docker Push (Nexus), no change
* CodeCov, no change
* Snyk Deps, no change
* Git tag
  * manual git tag using semver
* Sigul Sign, no change
* Semantic Release Final Steps
  * Changelog
  * Docker Hub retag and push
  * create and push release to github (mark as latest)

## Semver Only Approach
