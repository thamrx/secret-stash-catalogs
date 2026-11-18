# Secret Stash catalogs

Catalogs for Secret Stash, a PS4 catalog browser and installer.

| File | What it lists |
| --- | --- |
| [`homebrew.json`](homebrew.json) | Homebrew apps, emulators and open-source ports for PS4. Every package is downloaded straight from its developer's own GitHub release. Ports that need game data say so; bring your own. |

## How Secret Stash uses this repository

The Secret Stash catalog registry does not store catalogs. It keeps a link to a file in a GitHub repository like this
one, checks a few times a day for a new commit of that file, and validates it. When the new version is valid, the
registry records the commit and the file's SHA-256; the app then downloads the file from the pinned link
`https://raw.githubusercontent.com/<owner>/<repo>/<commit>/<path>` and checks the SHA-256. When it is not valid, the
registry keeps offering the last valid version and shows the maker what is wrong.

So a change here reaches the app only after it has been committed to `main` and the registry has checked it.

## The format in short

A catalog is one JSON document:

```json
{
  "format": "secret-stash-catalog",
  "version": 1,
  "titles": {
    "ABCD12345": {
      "content_id": "IV0000-ABCD12345_00-EXAMPLEAPP000000",
      "info": {
        "type": "app",
        "name": "Example App",
        "description": "What it does, in plain text.",
        "publisher": "its developer",
        "genres": ["Tools"]
      },
      "game": [
        {
          "version": "01.00",
          "size": 6619136,
          "sha256": "<64 hex characters>",
          "sources": [
            {"access": "direct", "urls": ["https://github.com/<developer>/<project>/releases/download/v1.0/app.pkg"]}
          ]
        }
      ]
    }
  }
}
```

- `titles` is keyed by title ID: four capitals and five digits.
- Each title has `game`, `updates` and/or `dlc`: lists of packages. A package is one `.pkg` file with one or more
  `sources`, and each source has `https://` links.
- `info` is the maker's own text, shown for titles the registry has no game information for (all homebrew).
- At most 20 MB. A field that the format does not describe is refused.

The full description, with every field and rule, is in the registry's documentation (`docs/FORMAT-AND-METADATA.md`
in the registry repository) and on the registry's Format page.

## Adding or changing an app

Open a pull request that changes the catalog file. Only homebrew: software whose developer publishes the `.pkg`
themselves. Give the developer's own release link, the package size and its SHA-256.
