# docums-monorepo-plugin

> **Note: This plugin is in beta.** Whilst it is not expected to significantly change in functionality, it may not yet be fully compatible with other Docums configuration and thus may break with some advanced configurations. Once these have been resolved and all bugs have been ironed out, we will move this to a stable release.

✚ This plugin enables you to build multiple sets of documentation in a single Docums. It is designed to address writing documentation in Spotify's largest and most business-critical codebases (typically monoliths or monorepos).

## Features

- **Support for multiple `docs/` folders in Docums.** Having a single `docs/` folder in a large codebase is hard to maintain. Who owns which documentation? What code is it associated with? Bringing docs closer to the associated code enables you to update them better, as well as leverage folder-based features such as [GitHub Codeowners].

- **Support for multiple navigations.** In Spotify, large repositories typically are split up by multiple owners. These are split by folders. By introducing multiple `docums.yml` files along with multiple `docs/` folder, each team can take ownership of their own navigation. This plugin then intelligently merges of the documentation together into a single repository.

- **Support across multiple repositories.** Using [Git Submodules](https://git-scm.com/book/en/v2/Git-Tools-Submodules) it is possible to merge documentation across multiple repositories into a single codebase dynamically.

- **The same great Docums developer experience.** It is possible to run `docums serve` in the root to merge all of your documentation together, or in a subfolder to build specific documentation. Autoreload still works as usual. No more using [symlinks](https://devdojo.com/tutorials/what-is-a-symlink)!

## Install

It's easy to get started using [PyPI] and `pip` using Python:

```terminal
$ pip install docums-monorepo-plugin
```

## Usage

Take a look at [our sample project](https://github.com/khanhduy1407/docums-monorepo-plugin/tree/master/sample-docs) or do the following:

- In the root, add the `monorepo` to your `plugins` key in `docums.yml`
- Create a subfolder, with a `docums.yml` with a `site_name` and `nav`, as well as a `docs/` folder with an `index.md`
- Back in in the root `docums.yml`, use the `!include` syntax in your `nav` to link to to a subfolder `docums.yml`

!!! info "Example root /docums.yml"
            
            site_name: Cats API

            # You can declare "!include" statements here. This enables you
            # to include docums.yml that are located in subfolders. In this
            # case we have two folders (v1/ and v2/) and wish to merge them
            # into this single navigation. The 'Intro' and 'Authentication'
            # files are located in the root docs/ folder as usual.

            nav:
              - Intro: 'index.md'
              - Authentication: 'authentication.md'
              - API:
                - v1: '!include ./v1/docums.yml'
                - v2: '!include ./v2/docums.yml'

            plugins:
              - monorepo

!!! info "Example submodule /v1/docums.yml"
            # In this case, we use the site_name to figure out how we should merge
            # this with the root documentation. It should refer to a folder structure.
            # The example below will merge documentation as following:
            #
            #   reference.md -> docs/versions/v1/reference.md -> http://localhost:8000/versions/v1/reference/
            #   changelog.md -> docs/versions/v1/changelog.md -> http://localhost:8000/versions/v1/changelog/
            #

            site_name: versions/v1

            nav:
              - Reference: "reference.md"
              - Changelog: "changelog.md"
            
            nav:
              - code-samples.md

!!! info "Example submodule /v2/docums.yml"
            
            # It works the same as above, but with relative to the site_name we use here:
            #
            #   migrating.md -> docs/versions/v2/migrating.md -> http://localhost:8000/versions/v2/migrating/
            #   reference.md -> docs/versions/v2/reference.md -> http://localhost:8000/versions/v2/reference/
            #   changelog.md -> docs/versions/v2/changelog.md -> http://localhost:8000/versions/v2/changelog/

            site_name: versions/v2

            nav:
              - Migrating to v2: "migrating.md"
              - Reference: "reference.md"
              - Changelog: "changelog.md"

An example filetree when using the Docums Monorepo plugin looks like this:

```terminal
$ tree .

├── docs
│   ├── authentication.md
│   └── index.md
├── docums.yml
├── v1
│   ├── docs
│   │   ├── changelog.md
│   │   └── reference.md
│   └── docums.yml
└── v2
    ├── docs
    │   ├── changelog.md
    │   ├── migrating.md
    │   └── reference.md
    └── docums.yml

5 directories, 10 files
```

## Supported Versions

- Python 3 &mdash; 3.6, 3.7
- [Docums] 1.0.0.0 and above.

## Extra Reading

- [docums][khanhduy1407/docums] on GitHub
- [Docums] documentation
- This was built using the [docums-plugin-template]

[khanhduy1407/docums]: https://github.com/khanhduy1407/docums
[docums-plugin-template]: https://github.com/khanhduy1407/docums-plugin-template
[pypi]: https://pypi.org
[docums]: https://khanhduy1407.github.io/docums
[github codeowners]: https://help.github.com/en/articles/about-code-owners
