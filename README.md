# How to turn in assignments
Download and zip your personal github project for the magento repository, please also include a link(s) to the git repository
# Assignment 1
## Goal : 
- Install Magento 
- Install a component 
  - _example_ : Add component for compliance with GDPR - Remaining requirements for GDPR can be reviewed *here*: [Dataflow Diagrams](https://devdocs.magento.com/guides/v2.3/architecture/gdpr/magento-2x.html#data-flow-diagrams)
## Method :
- PHP & Composer
## Description:
> Note : Common issue regarding finishing the prerequisites is that wrong php is used by default and thus composer. You can try using composer `--ignore-platform-reqs` and using the full path instead `php bin/magento`
1. [Get your authentication keys](https://devdocs.magento.com/guides/v2.3/install-gde/prereq/connect-auth.html)
2. [Get the metapackage](https://devdocs.magento.com/guides/v2.3/install-gde/composer.html#get-the-metapackage) 
```
composer create-project --repository=https://repo.magento.com/ magento/project-community-edition=2.3.2 --no-install  htdocs/m2
```
>Note : the folder is required to be empty
3. [Install the Magento application.](https://getcomposer.org/doc/03-cli.md#install-i)
```
composer install
```
>Note : you are required to change your current working directory to magento installation directory. 
3. [Install magento](https://devdocs.magento.com/guides/v2.3/install-gde/composer.html#install-magento) ( creates app/etc/env.php )
   - [Web Setup Wizard](https://devdocs.magento.com/guides/v2.3/install-gde/composer.html#web-setup-wizard)
   - [Command Line](https://devdocs.magento.com/guides/v2.3/install-gde/composer.html#command-line)
```
php bin/magento setup:install \
    --db-host=127.0.0.1:3306 \
    --db-user=root \
    --db-password=root \
    --db-name=magento2 \
    --admin-user=test \
    --admin-password=Test1234 \
    --admin-email=test@gmail.com \
    --admin-firstname=test \
    --admin-lastname=test \
    --backend-frontname=myadmin \
    --base-url=http://127.0.0.1/m2/ \
    --use-rewrites=1 \
    --enable-modules=1
```
4. [Enter developer mode](https://devdocs.magento.com/guides/v2.3/reference/cli/magento.html#deploymodeset)
```
php bin/magento deploy:mode:set developer
```
5. [Update Magento's composer.json Schema and the `require` property.](https://devdocs.magento.com/guides/v2.3/install-gde/install/cli/dev_add-update.html#add-a-require-section-to-composerjson) or [Use the `composer require` command to install](https://devdocs.magento.com/guides/v2.3/install-gde/install/cli/dev_add-update.html#use-the-composer-require-command-to-install)

```
composer require "mageplaza/module-gdpr":"1.2.3" --no-update
```
6. [Updating Magento's dependencies to their latest versions](https://getcomposer.org/doc/01-basic-usage.md#updating-dependencies-to-their-latest-versions)
```
composer update
```
7. [Clear the cache](https://devdocs.magento.com/guides/v2.3/reference/cli/magento.html#cacheclean)
```
php bin/magento cache:clean 
```
8. [Enable the module](https://devdocs.magento.com/guides/v2.3/reference/cli/magento.html#moduleenable)
```
php bin/magento module:enable Mageplaza_Core Mageplaza_Gdpr
```
9. [Update the database schema, data and import application configuration](https://devdocs.magento.com/guides/v2.3/reference/cli/magento.html#setupupgrade)
```
php bin/magento setup:upgrade
```
10. [Update the static content](https://devdocs.magento.com/guides/v2.3/reference/cli/magento.html#setupstaticcontentdeploy) 
```
php bin/magento -f setup:static-content:deploy
``` 
# Assignment 2 
## GOAL :
  - Apply code updates & install private packages
  - Patch third party components
## Method : 
- PHP & Composer & Git
## Description : 

### Minimal Requirements : 
- *Update magento module through composer using private repository*
> Note : The following examples is reusing the code of the previous install (of GDPR module) and you can ofcourse deviate from the examples below every step :see_no_evil: (as long as you complete the `GOALS`)
  
1. Create a new [remote](github.com/new) and local Repository
```
cd vendor/mageplaza/module-gdpr
git init
```
2. Create Tag in Local repository:
```
git tag 1.2.3
```
> note : the version after `tag` should be the same as the version specified in the [composer.json](https://devdocs.magento.com/guides/v2.3/extension-dev-guide/package/package_module.html#sample-composerjson-file)
3. [Create Tag in remote repository.](https://git-scm.com/docs/git-push#Documentation/git-push.txt---tags)
```
git push --tags
```
4. Instruct composer to look for packages in your own repository
```
composer config repositories.<an-unique-key> git git@github.com:<owner>/<repositoryname>
```
>Note : you are required to change your current working directory to magento installation directory. 
5. Update the dependencies 
```
composer update
```
### Advanced Requirements : 
- [*Create & Apply a patch for a Magento 2 Composer installation*](https://support.magento.com/hc/en-us/articles/360005484154-Create-a-patch-for-a-Magento-2-Composer-installation-from-a-GitHub-commit)

1. Install the cweagans/composer-patches plugin to apply patch to your Composer-based Magento 2 installation.
```
composer require cweagans/composer-patches
```
>Warning: Always perform comprehensive testing before deploying any unreleased patch.
2. Create a patches/composer directory in your local project.
```
mkdir -p patches/composer
```
3. save the current state of the package.
```
cd vendor/magento/framework
git init
git add -A
```
4. Make intended changes, i.e. Resolve this [issue](https://magento.stackexchange.com/a/252282), verify changes.
```
git diff
```
6. Add out put of `git diff` to a file in `<magento-installation-dir>/patches/composer/example.diff`.
```
git diff >> ../../../patches/composer/example.diff
```
> Note: You can ofcourse use any other way of saving the output of git diff to a file :see_no_evil:	
7. In the bottom of your composer.json, modify the key extra. Like so: 
```
"extra": {
    "composer-exit-on-patch-failure": true,
    "patches": {
        "magento/framework": {
            "WAMP/XAMP-issue: admin screen is full grey": "patches/composer/example.diff"
        }
    }
}
```
8. Test applying the patch.
```
composer -v install
```
> Note: the `-v` flag is so that you get a better failure message.
9. [Update only the composer.lock file](https://getcomposer.org/doc/03-cli.md#update-u)
```
composer update --lock 
```
10. ??? :sunglasses: :raised_hands: :pray: :clap: :metal: :muscle:

# FREE Magento educutaions
- [5 step : magento fundamentals](https://devdocs.magento.com/videos/fundamentals/)
- [20 step. frontend Tutorial](https://github.com/mcspronko/magento-2-pronko-consulting-theme)
- [Articles : deeper understand of Magento concepts](https://alanstorm.com/category/magento-2/)
- [Book : Master Magento 2](https://www.safaribooksonline.com/search/?query=Mastering%20Magento%202)
- [If you're still unsure of Magento here are the best presentations](https://firebearstudio.com/blog/the-best-magento-2-presentations.html)
- [Code snippets: If you want to get your hands dirty!](https://firebearstudio.com/blog/magento-2-developers-cookbook-useful-code-snippets-tips-notes.html)

# List of Windows Issues / Guides
- [install composer xampp](https://www.thecodedeveloper.com/install-composer-windows-xampp/) or [install magneto on xampp](https://hostadvice.com/how-to/how-to-install-magento-2-on-a-localhost-using-xampp/)
- [500 error or m2 folder is not visible in webroot](https://github.com/magento/magento2/issues/12777#issuecomment-352431790)
- [404 issues](https://magento.stackexchange.com/a/64808)
- [127.0.0.1/admin returns greyscreen](https://magento.stackexchange.com/a/252282)

### Additional Documentation for Git  
- [git init](https://git-scm.com/docs/git-init)
- [git status](https://git-scm.com/docs/git-status)
- [git add](https://git-scm.com/docs/git-add)
- [git commit](https://git-scm.com/docs/git-commit)
- [git push](https://git-scm.com/docs/git-push)
- [git pull](https://git-scm.com/docs/git-push)
- [git tag](https://git-scm.com/docs/git-tag)