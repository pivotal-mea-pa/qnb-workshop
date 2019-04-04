Account set up
==============

1.  Access to one of your organizations PCF installations is a pre-requisite for this lab.

Target the Environment
======================

1.  If you haven’t already, download the latest release of the Cloud
    Foundry CLI from <https://github.com/cloudfoundry/cli/releases> for
    your operating system and install it.

2.  Login to Pivotal Cloud Foundry:

        $ cf login -a https://{your_org_pcf_api_endpoint} ---sso

    Follow the prompts. A temporary token that serves as your password
    will be vended through single sign-on.

Apps Manager UI
===============

1.  An alternative to installing the CF CLI is via your PCF Apps Manager
    interface.

2.  Navigate in a web browser to https://{your_org_pcf_api_endpoint}

3.  Login to the interface with via the SSO option.

4.  Click the *Tools* link, and download the CLI matching your operating
    system
