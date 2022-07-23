# action-URL-watcher

Detect whether HTML contents is updated or not and commit diff to the repository.
Use repository to save the latest HTML file.

See [action.yml](action.yml).

## Inputs

| Name                | Description     | Default      | Required |
| ------------------- | --------------- | ------------ | -------- |
| `url`               | Target URL      |              | yes      |
| `excluded-patterns` | Ignore patterns | `""`         | no       |
| `save-dir`          | Saved directory | `./output`   | no       |
| `save-file`         | Saved file name | `latest.txt` | no       |

## Outputs

| Name   | Description                        |
| ------ | ---------------------------------- |
| `diff` | Whether the URL is updated or not. |

## Usage

```yaml
- uses: kokoichi206/action-URL-watcher@main
  with:
    url: https://odhackathon.metro.tokyo.lg.jp/

    # The patterns which are excluded in the target file.
    #  * Each patterns should be separated by `;`.
    #  * All expressions should be surrounded by `'`.
    #
    # Example(below):
    #  * This patterns match two regular expression.
    #    * "style.min.css\?[0-9]*-[0-9]*"
    #    * "common.js\?[0-9]*-[0-9]*"
    #  * `path/style.min.css?2022-0724` is treated as `path/`.
    excluded-patterns: '"style.min.css\?[0-9]*-[0-9]*";"common.js\?[0-9]*-[0-9]*"'

    save-dir: ./url_watcher_result
```

### Complete jobs usage

```yaml
jobs:
  action-check:
    name: Example
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v3

      - name: Check action
        id: my-action
        uses: kokoichi206/action-URL-watcher@main
        with:
          url: https://target.url.com
          excluded-patterns: '"style.min.css\?[0-9]*-[0-9]*";"common.js\?[0-9]*-[0-9]*"'
          save-dir: ./url_watcher

      - name: Action when diff found
        if: ${{ steps.my-action.outputs.diff }}
        run: |
          # Do something
          echo "Diff found!"
```

## Requirements

This action only works on ubuntu runner.

## License

This project is under the [MIT License](./LICENSE).
