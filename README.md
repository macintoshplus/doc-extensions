# PHP Third-Party Extension Documentation

This repository hosts documentation for third-party PHP extensions,
extensions maintained outside the [`php-src`](https://github.com/php/php-src)
repository (typically distributed via PECL or PIE).

It exists as a result of
[the RFC on separation of third-party extension documentation](https://wiki.php.net/rfc/third_party_ext_documentation),
and is intended to be rendered as part of the PHP manual at
[php.net/manual/extensions/](https://www.php.net/manual/extensions/).

## Contributing

Pull requests are welcome from anyone. Extension maintainers may additionally
request commit access.

Each extension lives in its own directory under `reference/`. Only the English
canonical documentation is maintained here; per the RFC, existing translations
were not carried over from the main manual.

For general guidance on the documentation format, refer to the
[contribution guidelines](https://doc.php.net/guide/contributing.md).

## Building With make and Docker

- Install Docker (https://docs.docker.com/get-docker/)
- Rebuild the documentation using `make`
- Open output/php-chunked-xhtml/ in your browser.

If the `doc-base` or `phd` repositories are available in directories to the
adjacent to this directory, those will be used for building.

To force the Docker image used for building to itself be rebuilt, you can run
`make -B build`, otherwise the `Makefile` will only build it if does not
already exist.

You can also build the `web` version of the documentation with `make php`
and the output will be placed in output/php-web

## Documentation pipeline

For more information on the various repositories that make up PHP's documentation pipeline,
see this [overview](https://github.com/php/doc-base/blob/master/docs/overview.md).

## Migration from doc-en

The history of this repository was extracted from [php/doc-en](https://github.com/php/doc-en)
with `git filter-repo`, preserving the full commit history (authors, dates, and messages)
of every migrated extension.
The final list of migrated extensions was settled in
[php/doc-extensions#12](https://github.com/php/doc-extensions/issues/12),
and the migration strategy in
[php/doc-extensions#13](https://github.com/php/doc-extensions/issues/13).

Key data points of the extraction:

- Migration notices (`MOVING_TO_DOC_EXTENSIONS_DO_NOT_CHANGE_HERE.md`) were added to doc-en
  in commit [`e2bd0429b5`](https://github.com/php/doc-en/commit/e2bd0429b5249cde95ca0c91964b86a0f3cbd607)
  (2026-09-02) **before** the extraction ran.
  Any change to a migrated extension in doc-en after that commit is discarded.
- The extraction ran on that commit's parent,
  [`5e26d11dc3`](https://github.com/php/doc-en/commit/5e26d11dc3f606b25c95467df22f9469636781f9)
  (2026-09-02), the last doc-en commit included in this repository's history.
- The paths given to `git filter-repo` cover the current `reference/` directories
  as well as historical locations (`reference/statistics`, the pre-2002 `functions/` chapters,
  and related appendices), so pre-restructure history is included.
- Build and repository scaffolding (CI, Docker, entities, and other root files)
  was carried over from doc-en with its history as well,
  and is adapted for this repository in follow-up commits.
- The mapping from original doc-en commit hashes to the rewritten hashes in this repository
  is recorded in [`migration/commit-map.txt`](migration/commit-map.txt).

The remaining migration steps, as agreed in
[php/doc-extensions#13](https://github.com/php/doc-extensions/issues/13):

1. Make this repository build: convert DTD entities to XML entities
   (using `doc-base/scripts/dtdent-conv.php`), with green CI on `make` as the goal.
2. Run a `docbook-cs` sweep over the repository to fix existing markup imperfections.
3. Publish this manual and set up redirects, in coordination with the infrastructure team.
4. Remove the migrated extensions from doc-en in batches,
   closing open doc-en PRs with a pointer and transferring issues where feasible.
