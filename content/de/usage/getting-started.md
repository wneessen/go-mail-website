---
title: Erste Schritte
weight: -20
---

Auf dieser Seite erfahren Sie, wie Sie mit dem Geekdoc-Theme beginnen können, einschließlich der Installation und der grundlegenden Konfiguration.

<!--more-->

{{< toc >}}

## Install requirements

You need a recent version of Hugo for local builds and previews of sites that use Geekdoc. As we are using [webpack](https://webpack.js.org/) as pre-processor, the normal version of Hugo is sufficient. If you prefer the extended version of Hugo anyway this will work as well. For comprehensive Hugo documentation, see [gohugo.io](https://gohugo.io/documentation/).

If you want to use the theme from a cloned branch instead of a release tarball you'll need to install `webpack` locally and run the build script once to create all required assets.

```Shell
# install required packages from package.json
npm install

# run the build script to build required assets
npm run build

# build release tarball
npm run pack
```
