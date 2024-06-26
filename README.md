# cxx-workflows [![CI](https://github.com/JeffersonLab/cxx-workflows/actions/workflows/ci.yml/badge.svg)](https://github.com/JeffersonLab/cxx-workflows/actions/workflows/ci.yml) <a href="https://codespaces.new/JeffersonLab/cxx-workflows"><img src="https://github.com/codespaces/badge.svg" height="20"></a>
GitHub action C/C++ workflows

## Reusable workflows

| Name                 | Description                      |
|----------------------|----------------------------------|
| [gh-pages-publish.yml](https://github.com/JeffersonLab/cxx-workflows/blob/main/.github/workflows/gh-pages-publish.yml) | Publish API docs to GitHub Pages |
| [gh-release.yml](https://github.com/JeffersonLab/cxx-workflows/blob/main/.github/workflows/gh-release.yml) | Create a GitHub Release |
| [unit-ci.yml](https://github.com/JeffersonLab/cxx-workflows/blob/main/.github/workflows/unit-ci.yml) | Build and run Unit tests (on Linux, Mac, and Windows) |

## How to use
This project uses it's own workflows in order to test them (the C++ App/Lib is just a demo/example).  Copy and paste one or more of the following files into your project `.github/workflows` directory and update parameters accordingly:

| Name                 | Description                      |
|----------------------|----------------------------------|
| [ci.yml](https://github.com/JeffersonLab/cxx-workflows/blob/main/.github/workflows/ci.yml) | Continuous Integration of an App/Lib |
| [cd.yml](https://github.com/JeffersonLab/cxx-workflows/blob/main/.github/workflows/cd.yml) | Continuous Deployment of an App/Lib with GitHub release |

The `ci` workflow invokes `unit-ci` to configure, build, and unit test.   The `ci` workflow can be customized with docker commands to launch containers and run integration tests ([Java Example](https://github.com/JeffersonLab/myquery/blob/e47681393f9a7a900dc1f0a932b6271bfa6356ed/.github/workflows/ci.yml#L20-L44])).

The `cd` workflow invokes `gh-release` and optionally `gh-pages-publish`.  The `gh-release` workflow uses the VERSION file to determine which tag to create.  The `cd` workflow generally should monitor the VERSION file for changes to trigger the workflow.  

The `gh-pages-publish` workflow creates docs on GitHub Pages.  For example, the demo Lib docs are here: [Doxygen API docs](https://jeffersonlab.github.io/cxx-workflows/).  The workflow creates a new directory in the [gh-pages](https://github.com/JeffersonLab/cxx-workflows/tree/gh-pages) branch of your project for each release using the semver name and copies the auto generated API docs there.  An index HTML with JavaScript can then use the GitHub API to lookup the directories in the branch and list them in the index.  This means there is a one-time setup for each project where you need to commit and push the index.html, index.js, and .nojekyll files.  The path to the docs are then obtained at `https://jeffersonlab.github.io/project/` where project is your project name.  Example [setup commit](https://github.com/JeffersonLab/cxx-workflows/commit/36de0f35037c3b14834bbfbbb9e7784f2e70eebe) - notice first line of index.js must be customized with project name.

## Demo App 
This project includes a `Hello World` C++ app to demonstrate the workflow.  Use the [cxx-devcontainer](https://github.com/JeffersonLab/cxx-devcontainer) to configure, build, and test:

```
cmake -S. -Bbuild
cmake --build build
ctest --test-dir build -C Debug -V
```

## Workflow Updates
Workflows are versioned in semver just as with regular software, however, the GitHub Action workflows convention is to reference a major version number such that backwards compatible minor and patch updates are received automatically.  This means a separate major tag such as `v1` must be moved after each release.  To move a major tag after a release execute (`v1` shown):

```
git tag -f v1
git push --tags -f
```

## See Also
- [Other workflows](https://github.com/search?q=org%3Ajeffersonlab+topic%3Agh-action-workflow&type=repositories)
