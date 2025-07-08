# conda-verify

GitHub action to verify a conda package by installing it with `micromamba` and ensuring it is importable by Python, and that the package version matches the expected value.

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
      uses: actions/upload-artifact
      with:
        name: artifact-conda-package
        path: ${{ env.PKG_NAME }}-*.conda
```



```yaml
jobs:
  conda-verify:
    needs: build
    run-on: ubuntu-latest
    defaults:
      run:
        shell: bash -el {0}
    steps:
      - name: Download conda package artifact
        uses: actions/download-artifact
        with:
          name: artifact-conda-package
          path: /tmp/local-channel/linux-64

      - name: Verify Conda Package
        uses: neutrons/conda-verify@main
        with:
          local-channel: /tmp/local-channel