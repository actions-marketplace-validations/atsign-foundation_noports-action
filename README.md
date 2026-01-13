<!-- pyml disable-num-lines 4 md013,md033-->
<h1><a href="https://atsign.com#gh-light-mode-only">
   <img width=250px src="https://atsign.com/wp-content/uploads/2022/05/atsign-logo-horizontal-color2022.svg#gh-light-mode-only" alt="The Atsign Foundation"></a>
<a href="https://atsign.com#gh-dark-mode-only">
   <img width=250px src="https://atsign.com/wp-content/uploads/2023/08/atsign-logo-horizontal-reverse2022-Color.svg#gh-dark-mode-only" alt="The Atsign Foundation"></a></h1>

# NoPorts Action

Use [NoPorts](https://www.noports.com/) within your GitHub Actions workflows.

This action installs NoPorts and optionally an atKeys file that will be used
to make a connection.

## Usage

### Basic example

This will install the latest release of the NoPorts binaries.

```yaml
name: NoPorts Example

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  test-noports:
    runs-on: ubuntu-latest # or ubuntu-24.04-arm
    steps:
      - name: Install NoPorts
        uses: atsign-foundation/noports-action@v0.0.1
          
      - name: Run a command
        run: sshnp --help
```

### Advanced example

This example specifies a specific release version of NoPorts and adds the
atKeys file for `@alice` that's stored in a GitHub Actions secret named
`ATKEYS_ALICE`.

It then starts an NoPorts tunnel to a daemon using the `@bob`
atSign via the Americas relay `@rv_am` to a device named `example123`
connecting local port 1234 to remote port 1234.

```yaml
name: NoPorts Example

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  noports-job:
    runs-on: ubuntu-latest
    steps:
      - name: Setup NoPorts and Authentication
        uses: atsign-foundation/noports-action@v0.0.1
        with:
          version: 'v5.14.2' # Optional: defaults to latest
          atsign: '@alice'
          atkeys-secret: ${{ secrets.ATKEYS_ALICE }}

      - name: Establish a NoPorts tunnel
        run: |
          # The action placed the keys in the default ~/.atsign/keys/ location
          npt -f alice -t bob -r rv_am -d example123 -l 1234 -p 1234
```

## Version History

### v0.0.2

* ' instead of " for passing atKeys file from input to file
* Correct flag for relay in advanced example

### v0.0.1

* Initial version.

## LICENSE

Licensed under the BSD 3 clause [LICENSE](LICENSE)

## Maintainers

This project was created by [@cpswan](https://github.com/cpswan/)
