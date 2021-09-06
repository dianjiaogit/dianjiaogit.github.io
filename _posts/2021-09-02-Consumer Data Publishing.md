---
layout: post
title:  "Consumer Data Publishing"
date:   2021-9-2 00:15:25 +0800
categories: [工作]
---

From README of Consumer Data, here are the processes we need to do when we publish to beta and live.

`cd fabric` goes to fabric/ directory first.

`source .envrc`, the purpose of this step is to set `POETRY_HTTP_BASIC_ARTIFACTORY_USERNAME` and `PASSWORD`. If it returns “Command not found” error, we can set them manually by running `portunus -r && source ~/.jfrog-env && export POETRY_HTTP_BASIC_ARTIFACTORY_USERNAME="${JFROG_USERNAME}" POETRY_HTTP_BASIC_ARTIFACTORY_PASSWORD="${JFROG_ACCESS_TOKEN}"`.

`poetry install`, this step will install all the necessary packages. Make sure poetry is installed before running this step.

`fab list_packages`, fab was installed in the last step, but only works on Python 2.7. So if it says “fab command not found“, we may need to switch the Python version by using `poetry env use python2.7`, and then go to the directory of python2.7 and run `source bin/activate`.

`fab --initial-sudo-password-prompt deploy:(latest|<revision>) -R beta-ops|live-ops|live-web`, and `fab --initial-sudo-password-prompt switch:<revision> -R beta-ops|live-ops|live-web`, release on beta-ops first, check whether the changes is working on beta-ops. Send a release note email before releasing on live-ops and live-web.

For checking on beta-ops, the server of beta-ops may need to restart by running `sudo -uconsumerdata-svc -H bash` and `/sbin/service wsgi restart`.
