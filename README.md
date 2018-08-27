# bundler-changelog

This repo exists to help automate the creation of [bundler](https://github.com/bundler/bundler)'s
CHANGELOG file.

It has a couple of different moving parts, each described below.
The tl;dr is that there's a script to ingest an existing CHANGELOG into `changes.csv`,
a script to open up all PRs that don't exist in the `changes.csv`
(to ease in adding entries for them),
and a script to generate a nice markdown file from the list of changes.

## releases.csv

A CSV file describing every Bundler release,
listing the versions released & release dates.

## changes.csv

This is the heart of this project -- a structured list of every change introduced into bundler!
Every entry contains:

- The PR associated with the change
  (this can be the only field with data if you don't want those changes to go into the CHANGELOG)
- The release of bundler that will _first_ contain this change (must be present in `releases.csv`)
- The section of the CHANGELOG the entry belongs in (usually `Features` or `Bugfixes`)
- The issues that this change is addressing (number only, pipe-delimeted)
- The authors of the change (GitHub usernames, pipe-delimited)
- The changelog message

## from_changelog

This script was used to seed the initial `changes.csv` and `releases.csv` file from
the Bundler repo. It shouldn't be necessary to use on a regular basis.

## changes

This script will take the changes in `changes.csv` and write them out to `./CHANGELOG.md`,
as well as normalizing all the CSV files in the repo.

## open_prs_since

This script will find every PR merged into `HEAD` since a given stable branch
(hard-coded into the script), and for each PR that does not have an entry in `changes.csv`,
will call `open $PR_URL`, and wait until `y` is given as input before proceeding to the next PR.
This is to make it easy to document every merged PR, and ensure no changes go undocumented,
while also allowing for incremental CHANGELOG writing (e.g. can be run any time, not just
immediately before a release).

Expected to be run inside of the Bundler repo.
