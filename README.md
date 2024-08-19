# Setup R Dependencies Action

## Description

A wrapper on [`r-lib/actions/setup-r-dependencies`](https://github.com/r-lib/actions/tree/v2-branch/setup-r-dependencies) with additional features as below:

- Use dependencies from identical feature branch (if exists). This allows to co-develop packages and test them together.
- Use development version of dependencies if specified in the `DESCRIPTION` file. This allows to test packages with dependencies that are not yet released.

This logic requires additional input of package references to check against. Please see the `lookup-refs` parameter description for more details. Currently, only GitHub references are supported.

The implementation relies on the `pkgdepends` package and it is temporarily changing the `DESCRIPTION` file by adding new `Config/Needs/` fields. Then `r-lib/actions/setup-r-dependencies` is called with the `needs` parameter.

## Action type
Composite

## Author
Insights Engineering

## Inputs
* `github-token`:

  _Description_: Token with permissions to clone repositories with dependencies.

  _Required_: `false`

  _Default_: `""`

* `repository-path`:

  _Description_: Directory where the R package is located relative to the calling GitHub workflow workspace.

  _Required_: `false`

  _Default_: `"."`

* `lookup-refs`:

    _Description_: List of [package references](https://r-lib.github.io/pkgdepends/reference/pkg_refs.html) to be used for the feature branch. Multiple entries in new lines or separated by commas. Only GitHub references are supported.

    _Required_: `false`

    _Default_: `""`

* `extra-deps`:

    _Description_: List of extra dependencies to check against feature branch existence. Multiple entries in new lines or separated by commas. It allows to control indirect dependencies. This requires an entry in the `lookup-refs` input.

    _Required_: `false`

    _Default_: `""`

* `dependencies`:

    _Description_: Passed to `r-lib/actions/setup-r-dependencies`.

    _Required_: `false`

    _Default_: `"all"`

* `extra-packages`:

    _Description_: Passed to `r-lib/actions/setup-r-dependencies`.

    _Required_: `false`

    _Default_: `""`

* `needs`:

    _Description_: Passed to `r-lib/actions/setup-r-dependencies`. The value will be amended by `DepsBranch` and `DepsDev` values.

    _Required_: `false`

    _Default_: `""`

* `skip-desc-branch`:

    _Description_: Skip branch lookup for dependencies specified in the `DESCRIPTION` file.

    _Required_: `false`

    _Default_: `false`

* `skip-desc-dev`:

    _Description_: Skip development version lookup for dependencies specified in the `DESCRIPTION` file.

    _Required_: `false`

    _Default_: `false`

* `skip-install`:

    _Description_: Skip installation of dependencies.

    _Required_: `false`

    _Default_: `false`

* `max-iter`:

    _Description_: Maximum number of iterations to resolve dependencies.

    _Required_: `false`

    _Default_: `50`


## Outputs

None

## Usage

```yaml
    steps:
      - name: Checkout repo
        uses: actions/checkout@v4

      - name: Setup R
        uses: r-lib/actions/setup-r@v2

      - name: Setup R dependencies
        uses: insightsengineering/setup-r-dependencies@v1
        with:
          github-token: ${{ secrets.REPO_GITHUB_TOKEN }}
          lookup-refs: |
            test/package1
            test/package2
            test/package3

      - name: Check R package
        uses: r-lib/actions/check-r-package@v2
```
