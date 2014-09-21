# JIRA/GitHub management IRCBot
[![Build Status](http://ci.jenkins-ci.org/view/Infrastructure/job/infra_ircbot/badge/icon)](http://ci.jenkins-ci.org/view/Infrastructure/job/infra_ircbot/)

This IRC bot sits on `#jenkins` as `jenkins-admin` and allow users to create/fork repositories on GitHub, etc. More info: [IRC Bot Wiki][1]

## Deployment
This repo is containerized, then deployed to our infrastructure via Puppet. 
You should have a Write permission to https://github.com/jenkins-infra/ircbot and https://github.com/jenkins-infra/jenkins-infra to deploy the new version of the Bot

Actions:

1. Commit/merge changes into the master branch
2. Wait till the preparation of Docker package on Jenkins INFRA 
 * Go to the [IRC Bot job][2] on https://ci.jenkins-ci.org
 * Wait till the automatic build finishes with a SUCCESS status
4. Modify the version on Puppet infrastructure
 *  Edit your <b>local fork</b> the following file: https://github.com/jenkins-infra/jenkins-infra/blob/staging/dist/profile/manifests/jenkinsadmin.pp  
 * Change the <code>$tag</code> variable in the <code>class profile::jenkinsadmin</code> class. 
   * Format: build${JENKINSCI_BUILD_NUMBER}
 * Create a pull request to the main repo. Branch=staging
 * Wait till the merge of the pull request
5. Wait till the deployment
 * See first steps of the deployment process on https://jenkins.ci.cloudbees.com/job/infra/job/jenkins-infra
 * The further deployment will be performed asynchronously (puppet checks for changes once per 15 minutes)
   * <code>jenkins-admin</code> will leave and join the chat
   * infra-butler will mention in #jenkins-infra that Spinach was updated

[1]: https://wiki.jenkins-ci.org/display/JENKINS/IRC+Bot
[2]: https://ci.jenkins-ci.org/job/infra_ircbot/