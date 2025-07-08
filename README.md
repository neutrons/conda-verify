# conda-verify

GitHub action to verify a conda package by installing it with `micromamba` and ensuring it is importable by Python, and that the version reported by conda and python match.

## Usage

This action assumes that you have built a `<package-name>.conda` built,  
and that it is located in a conda-style channel directory.

For example:

```yaml
jobs:
  build:
    steps:
    # steps to build your .conda package
    ...

    - name: Upload conda package as artifact
      uses: actions/upload-artifact@v4
      with:
        name: artifact-conda-package
        path: ${{ env.PKG_NAME }}-*.conda

  # Then, to verify the conda package:
  conda-verify:
    needs: build
    run-on: ubuntu-latest
    defaults:
      run:
        shell: bash -el {0}
    steps:
      - name: Download conda package artifact
        uses: actions/download-artifact@v4
        with:
          name: artifact-conda-package
          path: /tmp/local-channel/linux-64

      - name: Verify Conda Package
        uses: neutrons/conda-verify@main
        with:
          local-channel: /tmp/local-channel
          package-name: ${{ env.PKG_NAME }}
          extra-channels: mantid neutrons pyoncat
```

Available inputs are listed in [`action.yml`](action.yml).