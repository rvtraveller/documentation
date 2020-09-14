---
title: Build Tools
subtitle: Convert Existing Projects to the Build Tools Workflow
description: If you have an existing website not using a Composer workflow, use Build Tools to convert it.
cms: "Drupal"
categories: [develop]
tags: [updates, continuous-integration, composer, workflow]
buildtools: true
anchorid: update
type: guide
permalink: docs/guides/build-tools/convert/
image: buildToolsGuide-thumb
getfeedbackform: default
---
In this lesson, we'll convert an existing website to the Build Tools workflow.

## Reasons to Convert To Build Tools
Modern day websites depend on a variety of third party libraries, each with their own dependencies. Additionally, certain Drupal modules or WordPress plugins (such as the [Drupal Address module](https://www.drupal.org/project/address)) are required to be installed via Composer.

Leveraging Build Tools also sets you up to leverage [Automated Testing](/guides/build-tools/tests/) within your project to be sure your site continues functioning after every commit.

## Dependencies

The Build Tools Convert tool depends on the [Composerize Drupal](https://github.com/grasmash/composerize-drupal) and [Composerize WordPress](https://github.com/rvtraveller/composerize-wordpress) to convert your site to a Composer base. You'll also need `wget` and `tar` available on your local system.

## Convert Your Project

To convert your project, use the `terminus build:project:convert` command. The command takes 1 parameter, the Git URL to the website to be converted and support Git URLs from Pantheon, GitHub, BitBucket, and both public and private GitLab instances.

Similar to [creating a new project](/guides/build-tools/create-project/), a number of command line options are available to specify your Pantheon organization, Git Team, and preferred Git provider. Details about all of the command line options can be found in the [Build Tools Readme](https://github.com/pantheon-systems/terminus-build-tools-plugin).
