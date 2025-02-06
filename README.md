## TeX Live Docker

With this Dockerfile, you can create a Docker Image that holds a full texlive installation including Inkscape to include SVG Files inside your document.

## How to build

Just run

    docker buildx build --push --platform linux/arm64,linux/amd64 -t username/tag:release ./

while changing username, tag and release.

## What is the difference?

The full_installation uses the standard installation procedure and takes a while to build.

The prebuild_texlive folder however uses the texlive/texlive:latest docker image and just adds inkscape, which is way faster and much smaller image size.

## Pull directly

I pushed the prebuild_texlive one to my docker hub: https://hub.docker.com/r/timoxd7/texlive-svg
Feel free to use it.

## Usage

Just run the container with the command you want. For me (i started with Overleaf) the command is:

    docker run --rm -v ${PWD}:/src -w /src timoxd7/texlive-svg:2023 latexmk -pdf -shell-escape

Inside my Repository, i have a LatexMk file which does all the configuration.

## Auto-Release

In combination with Github Actions, you can just auto-create a pre-release by each push of your documents source to Github. Feel free to use this example:

```
name: Compile and release PDF

on: 
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
    - name: Checkout repository
      uses: actions/checkout@v3

    - name: Run Docker container
      run: docker run --rm -v ${PWD}:/src -w /src timoxd7/texlive-svg:2023 latexmk -pdf -shell-escape

    - name: Create Release
      id: create_release
      uses: actions/create-release@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }} # This token is provided by Actions
      with:
        tag_name: rel-${{ github.sha }}
        release_name: Release ${{ github.sha }}
        draft: false
        prerelease: true

    - name: Upload Release Asset
      id: upload-release-asset
      uses: actions/upload-release-asset@v1
      env:
        GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
      with:
        upload_url: ${{ steps.create_release.outputs.upload_url }} 
        asset_path: ./your_document_name.pdf  # Put here the path to your document
        asset_name: your_document_name_in_release.pdf # Set here the name of your document in releases
        asset_content_type: application/pdf
```

Don't forget to activate write access for actions inside Settings -> Actions of your repository. Note that this will create a tag inside your repository on each pushed commit!
