# GitHub Action to check if change log entry conforms to Astropy format

Check if a change log entry is present. If present, whether it is in the
expected section given the milestone. If not, whether it is allowed to
be missing or not. Create a `.github/workflows/check_changelog_entry.yml`
with this:

```
name: Check PR change log

on:
  pull_request_target:

jobs:
  changelog_checker:
    name: Check if change log entry is correct
    runs-on: ubuntu-latest
    steps:
    - name: Check change log entry
      uses: pllim/action-check_astropy_changelog@main
      env:
        CHANGELOG_FILENAME: CHANGES.rst
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

This action uses [astropy-changelog](https://github.com/astropy/astropy-changelog) to parse the change log.

Labels can be applied to the pull request to control its outcome:

* `skip-changelog-checks`: This results in success regardless; same as
  skipping the check altogether.
* `no-changelog-entry-needed`: This results in failure if a change log entry
  is present and success if it is missing. Use this label when the change log
  is decidedly not needed no matter what.
* `Affects-dev`: This results in failure if a change log entry is present and
  success if it is missing. This is because a change to an unreleased change
  does not require a change log.

Other ways this action can fail:

* Change log file is missing. This file is assumed to be `CHANGES.rst` but
  can be overwritten by `CHANGELOG_FILENAME` environment variable.
* Change log entry is present in multiple version sections.
* Change log entry is present but no milestone is set.
* Change log entry is under a version section that is inconsistent with the
  milestone set.
* Change log entry is missing and no special label (see above) is applied to
  indicate that it is expected to be missing.
