---
layout: post
title:  "Consumer Data Publishing"
date:   2021-9-2 00:15:25 +0800
categories: [工作]
---

cd fabric

`source .envrc` 
#目的是为了set POETRY_HTTP_BASIC_ARTIFACTORY_USERNAME和PASSWORD, 也可以通过`portunus -r && source ~/.jfrog-env && export POETRY_HTTP_BASIC_ARTIFACTORY_USERNAME="${JFROG_USERNAME}" POETRY_HTTP_BASIC_ARTIFACTORY_PASSWORD="${JFROG_ACCESS_TOKEN}"`手动设置

poetry install
fab list_packages
fab --initial-sudo-password-prompt deploy:(latest|<revision>) -R beta-ops|live-ops|live-web
fab --initial-sudo-password-prompt switch:<revision> -R beta-ops|live-ops|live-web
