## TeX Live Docker

With this Dockerfile, you can create a Docker Image that holds a full texlive installation including Inkscape to include SVG Files inside your document.

## How to build

Just run

    docker build -t username/tag:release ./

while changing username, tag and release.

## What is the difference?

The full_installation uses the standard installation procedure and takes a while to build.

The prebuild_texlive folder however uses the texlive/texlive:latest docker image and just adds inkscape, which is way faster and much smaller image size.
