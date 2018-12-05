# Requirements for Passing this course on webdevelopment for E-commerce Platforms
##Step 1 : Download Magento Package : 
```
composer create-project --repository=https://repo.magento.com/
magento/project-community-edition=2.2.6 <folder>
```
om det blir fel kommer inte metadata paketet installeras och då behövs composer install köras, 
```
cd <folder>
composer install 
```
## Step 2 Installera Magento Package
här efter måste magento installeras, det går att göra genom att besöka url:en eller via cli (cli exempel) : 
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
## Steg 3 Konfigurera Magento
här efter kommer lite "prestanda" inställningar till magento : 
```
php bin/magento config:set catalog/frontend/flat_catalog_product 1 && \
php bin/magento config:set catalog/frontend/flat_catalog_category 1 && \
php bin/magento config:set dev/js/minify_files 1  && \
php bin/magento config:set dev/css/minify_files 1
```
admin inställningar : 
```
php bin/magento config:set admin/security/use_form_key 0 && \
php bin/magento config:set admin/security/admin_account_sharing 1 && \
php bin/magento config:set admin/captcha/enable 0
```
generalla inställningar : 
```
php bin/magento config:set web/url/redirect_to_base 0 && \
php bin/magento config:set web/session/use_frontend_sid 0 && \
php bin/magento config:set dev/static/sign 0
```
## Steg 4 installera modul via Composer
Installera gdpr modul som tillåter användare att ta bort konto sitt från hemsidan.
```
composer require mageplaza/module-gdpr && \
php bin/magento setup:upgrade && \
php bin/magento module:enable Mageplaza_Core Mageplaza_Gdpr
```
## Steg 5 [Add new product Attribute](https://devdocs.magento.com/videos/fundamentals/add-new-product-attribute/) 
verifiera att detta fungera genom att gå in på admin. När du skapar en produkt ska du ett attribut finnas med flervals alternativ. I detta specifika tutorial så är det tyget av produkten. 
## Steg 6 How to : [Add a Javascript Module](https://devdocs.magento.com/videos/fundamentals/add-a-javascript-module/)
GDPR compliance, förutom att kunna ta bort konto information behöver vi meddela att vi b.la. använder cookies. ni får bli kreativa här för att visa en lösning, men det lättast sättet att uppnå så att cookie meddelandet är att ändra `HELLO WORLD!` till `vi använder cookies` i filen `view/frontend/templates/hello.phtml`. Samt för att detta ska dyka upp på alla sidor behöver vi ändra namnet på följande fil: `catalog_product_view.xml` till: `default.xml`. 
## Steg 7 Create your Theme, Use [this](https://github.com/mcspronko/magento-2-pronko-consulting-theme) as a boilerplate.
For further instructions watch the [first video part](https://www.youtube.com/watch?v=zdjSvVUYMJo) the specifics on how to register your own theme. After you have created theme.xml and registration.php you're done with the minimum requirements.
## Steg 8 Modify the Luma theme. 
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

# KRAV FÖR VÄLGODKÄNT 
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

#Inlämning : 
mail till `benjamin@nordicwebteam.se` med subject : `wie17 - <insert_your_full_name_here>`
innehåll på mailet ska vara, länk till er github repo 
samt lite åsikter/feedback kring kursen . 
Ex : Vad ni anser om upplägget, komplexiteten/upplysningarna av innehållet, samt vilka byggstenar, kompromisser eller brister som behöver diskuteras innan kursen påbörjas.

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
[List of Magento Common Issues and Solutions](https://firebearstudio.com/blog/magento-2-developers-cookbook-useful-code-snippets-tips-notes.html)
[List of Videos for Magento Fundamentals](https://devdocs.magento.com/videos/fundamentals/)
[Link to register for Awesome Tutorial](https://www.safaribooksonline.com/register/) []https://www.safaribooksonline.com/search/?query=Mastering%20Magento%202
https://firebearstudio.com/blog/the-best-magento-2-presentations.html

#For windows issue with images/css
https://magento.stackexchange.com/questions/64802/magento-2-404-error-for-scripts-and-css
