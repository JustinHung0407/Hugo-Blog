# Justin's Blog

[![Netlify Status](https://api.netlify.com/api/v1/badges/51c3e506-dad6-430f-b575-8a0e4c28c6e5/deploy-status)](https://app.netlify.com/sites/justinblog/deploys)

A really simple blog for recording things in daily life.

> Build by [Hugo](https://github.com/gohugoio/hugo) and Deploy on [Netlify](https://www.netlify.com/).

## Running Project
  * Local test
    * `hugo server --disableFastRender -w -v --baseUrl=localhost --bind="0.0.0.0"`
  * Docker:
    * `docker build -t hugo-app .`
    * `docker run -itd -p 80:80 hugo-app`
  * Kubernetes
    * Kustomize:
        * <https://github.com/kubernetes-sigs/kustomize>
        * `kustomize build k8s/base/overlays/production | kubectl apply -f -`
    * Skaffold:
        * <https://skaffold.dev/docs/>
        * `skaffold run -p development`
        * `skaffold dev`
        * `skaffold deploy`
    * Makefile:
        * <https://dev.to/yankee/streamline-projects-using-makefile-28fe>
        * `make kubectl-apply`

## Submodule Usage
- add submodule
  - `git submodule add https://github.com/adityatelange/hugo-PaperMod.git themes/PaperMod --depth=1`
- init submodule
  - `git submodule update --init --recursive`
- update submodule
  - `git submodule update --remote --merge`
- remove submodule
  - `git rm -r --cached themes/PaperMod`
  - `rm -rf .git/modules/themes/PaperMod`
  - `rm -rf themes/PaperMod`
  - edit .gitmodules
  - edit .git/config

## Syntax Highlighting
- Using PrismJS - dracula
  - `https://github.com/PrismJS/prism-themes/blob/master/themes/prism-dracula.css`
- Transform toool
  - `https://css2sass.herokuapp.com/`

## Reference
- [submodule](https://stackoverflow.com/questions/1260748/how-do-i-remove-a-submodule)