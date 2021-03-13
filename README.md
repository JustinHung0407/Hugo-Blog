# Justin-simple-blog [[Hugo version](https://github.com/JustinHung0407/Hugo-Blog)]

[![Netlify Status](https://api.netlify.com/api/v1/badges/51c3e506-dad6-430f-b575-8a0e4c28c6e5/deploy-status)](https://app.netlify.com/sites/justinblog/deploys)

A really simple blog for recording things

## Hugo local test scripts
* `hugo server --disableFastRender -w -v --baseUrl=localhost --bind="0.0.0.0"`

## Build project
* Docker:
  * `docker build -t hugo-app .`
  * `docker run -itd -p 80:80 hugo-app`

kustomize build k8s/base/ | kubectl apply -f -
## Build and Run Project
* Kustomize:
    * [https://github.com/kubernetes-sigs/kustomize]()
    * `kustomize build k8s/base/overlays/production | kubectl apply -f -`
* Skaffold:
    * [https://skaffold.dev/docs/]()
    * `skaffold run -p development`
    * `skaffold dev`
    * `skaffold deploy`
* Makefile:
    * [https://dev.to/yankee/streamline-projects-using-makefile-28fe]()
    * `make kubectl-apply`


## Submodule Usage
- add submodule
  - `git submodule add https://github.com/panr/hugo-theme-hello-friend.git themes/hello-friend`
  - `git submodule add https://github.com/rhazdon/hugo-theme-hello-friend-ng.git themes/hello-friend-ng`

- remove submodule
  - `git submodule deinit themes/hello-friend`
  - `rm -rf .git/modules/themes/hello-friend`
  - `git rm -f themes/hello-friend`
  - `git rm --cached themes/hello-friend`

## Reference
- [submodule](https://stackoverflow.com/questions/1260748/how-do-i-remove-a-submodule)
- 