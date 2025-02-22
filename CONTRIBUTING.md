# Development - Contributing

PGStac uses a dockerized development environment. However,
it still needs a local install of pypgstac to allow an editable
install inside the docker container. This is installed automatically
if you have set up a virtual environment for the project. Otherwise
you'll need to install a local copy yourself by running `scripts/install`.

To build the docker images and set up the test database, use:

```bash
scripts/setup
```

To bring up the development database:
```
scripts/server
```

To run tests, use:
```bash
scripts/test
```

To rebuild docker images:
```bash
scripts/update
```

To drop into a console, use
```bash
scripts/console
```

To drop into a psql console on the database container, use:
```bash
scripts/console --db
```

To run migrations on the development database, use
```bash
scripts/migrate
```

To stage code and configurations and create template migrations for a version release, use
```bash
scripts/stageversion [version]
```

Examples:
```
scripts/stageversion 0.2.8
```

This will create a base migration for the new version and will create incremental migrations between any existing base migrations. The incremental migrations that are automatically generated by this script will have the extension ".staged" on the file. You must manually review (and make any modifications necessary) this file and remove the ".staged" extension to enable the migration.

### Making Changes to SQL
All changes to SQL should only be made in the `/src/pgstac/sql` directory. SQL Files will be run in alphabetical order.

### Adding Tests
PGStac tests can be written using PGTap or basic SQL output comparisons. Additional testing is available using PyTest in the PyPgSTAC module. Tests can be run using the `scripts/test` command.

PGTap tests can be written using [PGTap](https://pgtap.org/) syntax. Tests should be added to the `/src/pgstac/tests/pgtap` directory. Any new sql files added to this directory must be added to `/src/pgstac/tests/pgtap.sql`.

The Basic SQL tests will run any file ending in '.sql' in the `/src/pgstac/tests/basic` directory and will compare the exact results to the corresponding '.sql.out' file.

PyPgSTAC tests are located in `/src/pypgstac/tests`.

All tests can be found in tests/pgtap.sql and are run using `scripts/test`

Individual tests can be run with any combination of the following flags "--formatting --basicsql --pgtap --migrations --pypgstac". If pre-commit is installed, tests will be run on commit based on which files have changed.


### To make a PR
1) Make any changes.
2) Make sure there are tests if appropriate.
3) Update Changelog using "### Unreleased" as the version.
4) Make any changes necessary to the docs.
5) Ensure all tests pass (pre-commit will take care of this if installed and the tests will also run on CI)
6) Create PR against the "main" branch.



### Release Process
1) Run "scripts/stageversion VERSION" (where version is the next version using semantic versioning ie 0.7.0
2) Check the incremental migration created in the /src/pgstac/migrations file with the .staged extension to make sure that the generated SQL looks appropriate.
3) Run the tests against the incremental migrations "scripts/test --migrations"
4) Move any "Unreleased" changes in the CHANGELOG.md to the new version.
5) Open a PR for the version change.
6) Once the PR has been merged, start the release process.
7) Create a git tag `git tag v0.2.8` using new version number
8) Push the git tag `git push origin v0.2.8`
9) The CI process will push pypgstac to PyPi, create a docker image on ghcr.io, and create a release on github.


### Get Involved

Issues and pull requests are more than welcome: https://github.com/stac-utils/pgstac/issues
