---
title: Integrated Composer
description: Learn how to deploy a Drupal site with integrated Composer
tags: [composer, workflow]
categories: [drupal]
contributors: [edwardangert]
searchboost: 150
reviewed: "2020-07-21"
---

Pantheon is testing a release of Drupal with integrated Composer and is opening the feature to users for Limited Availability testing. Integrated Composer lets you deploy your site on Pantheon with one-click updates for both upstream commits and Composer dependencies, while still receiving upstream updates.

## Get access to Integrated Composer

Contact [Support](/support). If you are not part of an organization already, your CSE will add you to a test organization from which you can deploy the upstream. If you are already part of an organization, the feature can be enabled organization-wide.

## Create a Drupal or WordPress Site with Integrated Composer

1. [Fork the Pantheon-maintained repository](/create-custom-upstream#create-and-host-the-repository-remotely):
   - **Drupal**: [https://github.com/pantheon-upstreams/drupal-project](https://github.com/pantheon-upstreams/drupal-project).
   - **WordPress**: [https://github.com/pantheon-upstreams/wordpress-project](https://github.com/pantheon-upstreams/wordpress-project).
1. [Add a new Custom Upstream](/create-custom-upstream#connect-repository-to-pantheon) on the Pantheon dashboard.
1. Create a new Drupal 8 or WordPress site from the upstream to confirm that it’s working.
   - Do not customize the upstream yet.
1. In the Dev environment, click <Icon icon="new-window-alt" text="Visit Development Site"/> and follow the prompts to complete the CMS installation.
1. [Clone the site locally](/local-development#get-the-code) and run `composer install`.

## Upstream and Site Structure

The upstream has the following directory structure:

```none:title=core/
core/
├─ .gitignore
├─ composer.json
└─ pantheon.upstream.yml
├─ README.md
└─ upstream-config/
   └─ composer.json
```

- `.gitignore`: Prevents build artifacts generated by Composer from being committed to the upstream or site code repositories.
- `composer.json`: The two different `composer.json` files allow customization of individual sites without inherent merge conflicts and enable one-click updates.
  - Root-level: Site-level customizations.
  - `upstream-config/composer.json`: Customizations for the upstream
- `pantheon.upstream.yml`: The `build_step_demo: true` directive in `pantheon.upstream.yml` enables the build step. This name is temporary and will change.

When a site is created, Pantheon runs `composer install`, generates a `composer.lock` file and commits it back to the site’s code repository.

Build artifacts are stored in a Git tag (`pantheon_build_artifacts_master`), not the main branch (`master` for Dev or a Multidev feature branch).

## How to Add Dependencies to Your Upstream

1. Start with the local clone of your upstream repository you created above.
1. Change into the `upstream-config` directory.

  ```bash{promptUser: user}
  cd upstream-config
  ```

1. Run:

  ```bash{promptUser: user}
  composer require drupal/pkg-name --no-update
  ```

   - `--no-update` makes Composer run faster

1. Confirm the current configuration version:

  ```bash{outputLines:2}
  composer config version
  1.0.0
  ```

1. Increment the config version number. If you don't increment the version number, updated dependencies will be ignored. Replace `x.y.z` in this example with something like `1.0.1`:

  ```bash{promptUser: user}
  composer config version x.y.z
  ```

1. Commit and push.

## Applying One-click Updates

Navigate to **Code** in the Dev tab of the site's Dashboard.

Click **Check Now**. If updates are available, click **Apply Updates**.

## Add a Dependency to an Individual Site

1. Clone the git repository from the Pantheon site's dashboard.
1. Run `composer install`:

  ```bash{promptUser: user}
  composer install
  ```

1. Add a new dependency locally:

  ```bash{promptUser: user}
  composer require drupal/pkg-name
  ```

1. Commit `composer.json` and `composer.lock` and push.
   - Pantheon will run Composer, generate build artifacts, and deploy it to your Dev or Multidev environment.
1. Remove dependencies:

  ```bash{promptUser: user}
  composer remove
  ```

## Troubleshooting / FAQ

### What Composer commands does Pantheon run?

- Check for and applying updates:

  ```bash{promptUser: user}
  composer --no-cache --no-interaction --no-progress --prefer-dist update
  ```

- On all builds except applying updates:

  ```bash{promptUser: user}
  composer --no-cache --no-interaction --no-progress --prefer-dist install
  ```

### How do I view Composer's changes?

Use `git diff` to view changes, excluding composer.lock

```bash{promptUser: user}
git diff d94d1a1179 -- . ':(exclude)composer.lock'
```

Try [composer-lock-diff](https://github.com/davidrjonas/composer-lock-diff) to see what packages have changed after `composer update`.