# Requirements for Passing this course on web-development for E-commerce Platforms
## Step 1 : Download Magento Package : 
```
composer create-project --repository=https://repo.magento.com/
magento/project-community-edition=2.2.6 <folder>
```
If you get dependencies error you have to change directory directory and retry by install the package through composer, and you might need to use the parameter `--ignore-platform-reqs` everytime you use the command `composer`.
```
cd <folder>
composer install --ignore-platform-reqs
```
## Step 2 Install the Magento Package
To install magento you have to options, using the GUI which is accessible by simply visiting the website or by the CLI : 
```
php bin/magento setup:install \
    --db-host=127.0.0.1:3306 \
    --db-user=root \
    --db-password=root \
    --db-name=wie17 \
    --admin-user=test \
    --admin-password=Test1234 \
    --admin-email=test@gmail.com \
    --admin-firstname=test \
    --admin-lastname=test 
```
## Step 3 Configure Magento
Some basic performance settings : 
```
php bin/magento config:set catalog/frontend/flat_catalog_product 1 && \
php bin/magento config:set catalog/frontend/flat_catalog_category 1 && \
php bin/magento config:set dev/js/minify_files 1  && \
php bin/magento config:set dev/css/minify_files 1
```
Some admin settings : 
```
php bin/magento config:set admin/security/use_form_key 0 && \
php bin/magento config:set admin/security/admin_account_sharing 1 && \
php bin/magento config:set admin/captcha/enable 0
```
General Settings : 
```
php bin/magento config:set web/url/redirect_to_base 0 && \
php bin/magento config:set web/session/use_frontend_sid 0 && \
php bin/magento config:set dev/static/sign 0
```
## Step 4 Install Module Through Composer
Install a module for completing compliance for GDPR. 
```
composer require mageplaza/module-gdpr && \
php bin/magento setup:upgrade && \
php bin/magento module:enable Mageplaza_Core Mageplaza_Gdpr
```
## Step 5 [Add new product Attribute](https://devdocs.magento.com/videos/fundamentals/add-new-product-attribute/) 
Verify that this works by accessing the admin. When creating a product, you should have an attribute with multiple choice options. In this specific tutorial, it is the fabric of the product. 
## Step 6 [Add a Javascript Module](https://devdocs.magento.com/videos/fundamentals/add-a-javascript-module/)
 In addition to being able to remove account information, we need to inform the users that we may uses cookies. I want you to be creative and figure out solutin that is better, but the easiest way to accomplish that the cookie message is to change `HELLO WORLD !` to` we use cookies` in the `view / frontend / templates / hello.phtml` file. And for this to appear on all pages, we need to change the name of the following file: `catalog_product_view.xml` to:` default.xml`.
## Step 7 Create your Theme, Use [this](https://github.com/mcspronko/magento-2-pronko-consulting-theme) as a boilerplate.
For further instructions watch the [first video part](https://www.youtube.com/watch?v=zdjSvVUYMJo) the specifics on how to register your own theme. After you have created theme.xml and registration.php you're done with the minimum requirements for creating a theme. Before you continue to next step, the files from github repo in the former link in this step includes both `theme.xml` and `registration.php` These are basically correct except, unless we want to follow almost all of his videos linked in the github, therefor, we should change the <parent>Magento/luma</parent>    
## Step 8 Modify the Luma theme. 
Create this file `app/design/frontend/YourCompany/your_theme/Magento_Theme/layout/default.xml`
and add this content : 
```
<?xml version="1.0"?>
<page layout="1column" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:View/Layout/etc/page_configuration.xsd">
    <body>
        <referenceBlock name="form.subscribe" remove="true" />
    </body>
</page>
```
After you have installed a theme use this command generate configuration that needs to be committed as well. 
```
php bin/magento app:config:dump themes
``` 
The above will tell me the name of your theme and that it is sucessfully recognized by magento. 

# Requirements for "Väl Godkänt"
You need to follow common practices described for magento described in more detail [here](https://devdocs.magento.com/guides/v2.2/ext-best-practices/bk-ext-best-practices.html)
I will check most and foremost if you're modules are following the a [PSR-4 Compliant](http://www.php-fig.org/psr/psr-4/) file structure 
If that link doesn't make sense, here is the TL;DR:
Every module needs a composer.json and to be specific it needs to have this : 
```
 "autoload": {
    "files": [ "registration.php" ],
    "psr-4": {
      "NWT\\KCO\\": ""
    }
  }
```
And [here](https://magento.stackexchange.com/a/174728) is how to setup an automatic check on every commit.

#How to submit : 
mail me at: `benjamin@nordicwebteam.se` with subject : `wie17 - <insert_your_full_name_here>`
The content should include : 
constructive feedback regarding me or the course. 
Example #1: What are you're thoughts about the layout, the complexity and the information of the content presumed to be in this course and what was actually tought in this course. 
Example #2: What are the building blocks, compromises or shortcomings that need to be discussed before the course begins.

# HELP Resources
Some basic Magento developer commands are : 
```
rm -rf var/cache/* var/log/* var/page_cache/* var/session/* var/view_preprocessed/* pub/static/* generated/* && \
php bin/magento deploy:mode:set developer && \ 
php bin/magento setup:upgrade
```
Some times this doesn't show us any errors, all we see is a blank white screen, then the following commands might help out : 
```
rm -rf var/cache/* var/log/* var/page_cache/* var/session/* var/view_preprocessed/* pub/static/* generated/* vendor/* && \
composer install && \
php bin/magento setup:upgrade && \
php bin/magento setup:di:compile && \
php bin/magento setup:static-content:deploy -f && \
php bin/magento cache:flush && \
php bin/magento deploy:mode:set developer
```
Sometimes we get an error message `There are no commands defined in the the "xxxxxx" namespace.`
And to get a better understanding of what caused the issue run : 
```
php bin/magento list
```
Some basic Git commands are:
```
git status
git add
git commit
git push
git pull
```

Fetching your own repository through composer : 
```
composer config.repositories.namn vcs git@github.com:<username>/repo
composer require <vendor>/<module>
``` 
där vendor/module är som det står i den composer.json

# USEFUL LINKS 
 - [List of Magento Common Issues and Solutions](https://firebearstudio.com/blog/magento-2-developers-cookbook-useful-code-snippets-tips-notes.html)

- [List of Videos for Magento Fundamentals](https://devdocs.magento.com/videos/fundamentals/)

 - [Link to register for Awesome Tutorial](https://www.safaribooksonline.com/register/) 
 - [Link to view the Awesome Tutorial ]https://www.safaribooksonline.com/search/?query=Mastering%20Magento%202
 - [If you're still unsure of Magento here are the best presentations](https://firebearstudio.com/blog/the-best-magento-2-presentations.html)

# post scriptum
[Link to a fix For a problem only Windows Users had](https://magento.stackexchange.com/questions/64802/magento-2-404-error-for-scripts-and-css)
