# Github-deploy-actions

This action will auto build and deploy to target branch when it get triggered.

Also it can preserve the history of gh-pages and convenient for rolling back to
previous version.

And it will compare deployment file to previous version by using
`git status --porcelain`, it will not to deploy if nothing change.

# How to Use

```yml
name: deploy

on:
    push:
        branches:
            - master

jobs:
    build-and-deploy:
        runs-on: ubuntu-latest
        steps:
            - name: Checkout
              uses: actions/checkout@master

            - name: Build and Deploy
              uses: ./.github/actions/deploy
              env:
                  COMMIT_EMAIL: jeoy_z@126.com
                  COMMIT_NAME: jeoy
                  ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
                  BASE_BRANCH: master
                  DEPLOY_BRANCH: gh-pages
                  BUILD_SCRIPT: yarn && yarn build
                  FOLDER: build
```

## Environment variable

| param         |             description              | required |          default |
| :------------ | :----------------------------------: | -------: | ---------------: |
| COMMIT_NAME   | The name who commit this deployment  |    false | \${GITHUB_ACTOR} |
| COMMIT_EMAIL  | The email who commit this deployment |     true |                - |
| ACCESS_TOKEN  |     github token can acess repo      |     true |                - |
| BASE_BRANCH   |     The branch you want to build     |    false |           master |
| DEPLOY_BRANCH |    The branch you want to deploy     |    false |         gh-pages |
| BUILD_SCRIPT  | e.g. `npm install && npm run build`  |     true |                - |
| FOLDER        | The folder generated by build script |     true |                - |

# How It Works

When push to `master` branch

This Action will run `yarn && yarn build`

Then push `build` folder as a new commit on `gh-pages` branch

> note: mark sure `build` folder is on your gitignore list

# deploy page:

[gh-pages demo](https://jeoy.github.io/github-deploy-actions/)

## what exactly is done during the action

checkout this
[entrypoint.sh](https://github.com/jeoy/github-deploy-actions/blob/master/entrypoint.sh)
