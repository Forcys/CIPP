---
id: contributing
title: Contributing.
description: What you'll need to help develop the CIPP React frontend.
slug: /contributing
---

<!-- markdownlint-disable MD033 -->

Contributions to CIPP are welcome by everyone. There's a couple of things to keep in mind;

* These repositories are going through rapid changes. Every pull request should update version_latest.txt with versioning that follows [Semantic Versioning](https://semver.org)
* Speed and Security are two of our pillars, if it isn't fast, it isn't good, and if it isn't secure, it can't be accepted :)
* We try to use native APIs over Powershell Modules. Powershell modules tend to slow the entire processing. We currently only have Az.Keyvault and Az.Accounts loaded and prefer to keep it that way.
* The interface is made entirely in Bootstrap and Jquery. For Datatables we use the JQuery Datatables plugin.
* Avoid adding a deploy YML to your development repo. We'll remove those, but it's just an annoyance. If you want to both deploy and develop it's better to create two instances of the repo.

When contributing, or planning to contribute, please create an issue in the tracker [here](https://github.com/KelvinTegelaar/CIPP/issues). If you are fixing a bug, file a complete bug report and assign it to yourself, if you are adding a feature, please add "Feature Request" to the title and assign it to yourself.

## Feature requests

Feature requests that request integration with anything but M365 will be closed. We're not integrating directly with third party products until version 2.0. Pull requests that have integration components will be discussed and evaluated on a case-by-case basis.

## Pull Requests

We do not accept PRs or commits against Master. Master is always the final version. For both CIPP and CIPP-API we have at least two branches. Dev and Master. Please make any PR against Dev, when Dev is promoted to final we'll PR that against master.

<Details>
<Summary>## Naming Standards</Summary>

We follow a naming standard, as based on the name a user might get access to an API or not. Our current naming standard is as follows;
ListBla - Everything that generates a list (users)
EditBla - Anything that edits an exisiting object (edit user)
AddBla - Anything that adds an object (add user)
RemoveBla - Anything that deletes or removes an object (remove user)
ExecBla - Anything that executes an action (send mfa request to user)

</Details>

## Creating two instances

* Make a clone of your forked repo
* Optional: mark this repo as private
* Add the following github action, this will sync the repos every hour:

```YML
# This is a basic workflow that is manually triggered

name: Pull from master schedule

# Controls when the action will run. Workflow runs when manually triggered using the UI
# or API.
on:
  schedule:
    # * is a special character in YAML so you have to quote this string
    - cron:  '0 * * * *'
    # Inputs the workflow accepts.
# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  repo-sync:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
      with:
        persist-credentials: false
    - name: repo-sync
      uses: repo-sync/github-sync@v2
      with:
        source_repo: "KelvinTegelaar/CIPP"
        source_branch: "master"
        destination_branch: "master"
        github_token: ${{ secrets.PAT }}
```

* Go to settings of the repo
* click on add secret
* secret name "PAT"
* Secret value: a self created [personal access token](https://github.com/settings/tokens)