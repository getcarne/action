<p align="center">
  <img src="https://github.com/getcarne/carne/blob/main/.github/carne_logo.png" alt="Carne Logo" width="140px" height="140px"><br>
  Setup <a href="https://github.com/getcarne/carne">Carne</a> in GitHub Actions<br>
  Installs the Carne JavaScript runtime from <code>getcarne/carne</code> releases on Linux and macOS runners.
</p>

> [!NOTE]
> Windows is not supported (Carne publishes no Windows release binary).

## Usage

```yaml
- uses: getcarne/action@v1
  with:
    carne-version: latest
- run: carne --version
```

## Inputs

| Input | Default | Description |
|---|---|---|
| `carne-version` | `latest` | Version to install: `latest`, `0.1.2`, or `v0.1.2` |
| `arch` | `auto` | `auto`, `x86_64` (`x64`, `amd64`), or `arm64` (`aarch64`) |
| `token` | `github.token` | Token for latest-version API lookup |

## Outputs

| Output | Description |
|---|---|
| `version` | Resolved version without leading `v` (or `latest` when the API was unreachable) |
| `bin-path` | Directory containing the `carne` binary |

## Examples

Pinned version:

```yaml
- uses: getcarne/action@v1
  with:
    carne-version: 0.1.2
```

Leading `v` also works:

```yaml
- uses: getcarne/action@v1
  with:
    carne-version: v0.1.2
```

Override architecture:

```yaml
- uses: getcarne/action@v1
  with:
    arch: arm64
```

Use the outputs:

```yaml
- uses: getcarne/action@v1
  id: carne
  with:
    carne-version: 0.1.2
- run: echo "${{ steps.carne.outputs.version }} at ${{ steps.carne.outputs.bin-path }}"
```

## Notes

- Assets are `carne_<os>-<arch>.tar.gz` from
  `https://github.com/getcarne/carne/releases` (see the release matrix in
  `carne/.github/workflows/release.yml`).
- On Linux runners the action installs `libjavascriptcoregtk-4.1-dev` and
  `libssl-dev` via `apt-get`, mirroring the Carne release build.
- On Windows the action fails with a clear error.
