# cosign Github Action

## Goal

Easy to use, no fuzz. Drop-in and done.

## Usage example
### With manually generated key (through `cosign generate-key-pair`)

Requires secrets named `COSIGN_PRIVATE_KEY` and `COSIGN_KEY_PASSWORD` to be present in your repository.

```yaml
jobs:
  build-push:
    runs-on: ubuntu-latest

    steps:
    # Here you build and push your image
    # And some more voodoo

    - name: Sign image
      uses: tamcore/cosign-action@master
      env:
        COSIGN_PRIVATE_KEY: ${{ secrets.COSIGN_PRIVATE_KEY }}
        COSIGN_PASSWORD: ${{ secrets.COSIGN_KEY_PASSWORD }}
      with:
        tags: |
          ${{ steps.docker_metadata.outputs.tags }}
```
### Sign with Github OIDC token

Requires the job to be executed with `id-token: write` permission.
```yaml
jobs:
  build-push:
    runs-on: ubuntu-latest

    permissions:
      contents: read
      packages: write
      id-token: write # required for keyless signing

    steps:
    # Here you build and push your image
    # And some more voodoo

    - name: Sign image
      uses: tamcore/cosign-action@master
      with:
        cosign-keyless: true
        tags: |
          ${{ steps.docker_metadata.outputs.tags }}
```
