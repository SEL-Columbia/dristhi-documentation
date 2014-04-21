---
layout: page
title: "How to update Enketo on Dristhi app"
---

# How to update Enketo on Dristhi app

## Setup Enketo-Dristhi
1. Git checkout [Enketo-Dristhi][1] repository as as sibling to Dristhi app
2. Follow the steps mentioned in Enketo-Dristhi's [Readme.md][2] to setup Enketo-Dristhi to run locally

## Update Enketo-Dristhi
1. To update Enketo, first update Enketo-Dristhi by running `git pull --rebase`
2. Update all the git submodules by running `git submodule update --init --recursive`
3. Compile/build Enketo-Dristhi by running `grunt` at the root of the repository. This will generate a 'build' directory at the root of the repository

## Update Enketo-Dristhi on Dristhi app
1. In Dristhi app, enketo can be located at <app_root>/dristhi-app/assets/www/enketo. Delete this directory by running `rm -rf <app_root>/dristhi-app/assets/www/enketo`
2. Copy the contents of <enketo_dristhi_root>/build directory to <app_root>/dristhi-app/assets/www/enketo by running the following command from <app_root>, `cp -r ../<enketo_dristhi_root>/build dristhi-app/assets/www/enketo`
3. Delete the index.html file as it is not used in Dristhi app by running the following command from app_root, `rm dristhi-app/assets/www/enketo/index.html`

[1]: https://github.com/MartijnR/enketo-dristhi