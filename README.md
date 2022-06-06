# ensure-scripts
Scripts used in repos the ensure that dependencies are met.

This repo is a collection of ensure-* scripts with minimal dependencies to build development environment [go scripts](https://www.thoughtworks.com/insights/blog/praise-go-script-part-i) (or [scripts to rule them all](https://github.com/github/scripts-to-rule-them-all)) for projects. Each script ensures that a condition is met. If the script has dependencies, it ensures those dependencies are met, often by calling other ensure scripts.

## To use this repo to build a go script for another repo

From the local repo directory for a project that you're building a go script for: `git submodule add git@github.com:calef/ensure-scripts.git`

When you create your go script within your repo, you can then use the various ensure-scripts scripts from within your go script. One would use an ensures script within a go script as follows:

    cd ensure-scripts
    ./ensure-homebrew
    cd ..

If you find yourself building significant logic within your go script, consider if any of that should be extracted into ensure-scripts for reuse by other repos. However, this is *not a repo for bootstrapping every other repo* because go scripts should be revised with the repos they support. Do not put your repo-specific logic within this repo.

## To add or update ensures in ensure-scripts

    cd ~/projects
    git clone git@github.com:calef/ensure-scripts.git

Create or edit ensure-* scripts.

The ensure conventions are:

1. Every script is idempotent.
2. Every script ensures that its dependencies (frequently by calling other ensure scripts) are installed before ensuring a new dependency.
3. If a dependency cannot be met, then the ensure script should provide error output and return a non-zero exit code. Bash example: `echo "Must be running MacOS!" 1>&2; exit 1;`
4. For manual steps, which should be rare, provide error output. The ensure script should fail with a non-zero exit code (as in the case when a dependency cannot be met) so that the user can take the manual step and rerun the go script.
