# KRAV FÖR GODKÄNT 
Steg 1 : Installera Magento : 
```
composer create-project --repository=https://repo.magento.com/
magento/project-community-edition=2.2.6 <folder>
```
om det blir fel kommer inte metadata paketet installeras och då behövs composer install köras, 
```
cd <folder>
composer install 
```
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
Installera gdpr modul som tillåter en att ta bort konto från databasen : 
```
composer require mageplaza/module-gdpr && \
php bin/magento setup:upgrade && \
php bin/magento module:enable Mageplaza_Core Mageplaza_Gdpr
```
How to : [Add new product Attribute] (https://devdocs.magento.com/videos/fundamentals/add-new-product-attribute/) 

GDPR compliance, förutom att kunna ta bort konto information behöver vi meddela att vi b.la. använder cookies. ni får bli kreativa här för att visa en lösning, men för att uppnå detta måste det synas oavset på vilken url man går in på så är minimum att det ska stå att vi använder cookies.
How to : [Add a Javascript Module](https://devdocs.magento.com/videos/fundamentals/add-a-javascript-module/)
sedan ändra `catalog_product_view.xml` -> `default.xml`, samt `helloworld!` -> `vi använder cookies`

Use [this](https://github.com/mcspronko/magento-2-pronko-consulting-theme) github as a boilerplate for creating your own theme, watch the [first video part](https://www.youtube.com/watch?v=zdjSvVUYMJo) for further instructions on the specifics on how to register your own theme.
Or download a third party theme. 

After this is done you need to remove the newsletter from the footer of the magento 2 luma theme.

How to [Remove Newsletter](http://lmgtfy.com/?q=remove+newsletter+from+footer+magento+2) 
[click here](https://magento.stackexchange.com/questions/164340/how-to-remove-subscribe-field-from-luma-footer)

After you have installed a theme use this command generate configuration that needs to be committed as well. 
```
php bin/magento app:config:dump themes
``` 
The above will tell me the name of your theme and that it is sucessfully recognized by magento. 

# KRAV FÖR VÄLGODKÄNT 
You need to follow common practices described for magento described in more detail [here](https://devdocs.magento.com/guides/v2.2/ext-best-practices/bk-ext-best-practices.html)
I will check most and foremost if you're modules are following the a [PSR-4 Compliant](http://www.php-fig.org/psr/psr-4/) file structure 

#Inlämning : 
mail till `benjamin@nordicwebteam.se` med subject : `wie17 - <insert_your_full_name_here>`
innehåll på mailet ska vara, länk till er github repo 
samt lite åsikter/feedback kring kursen . 
Ex : Vad ni anser om upplägget, komplexiteten/upplysningarna av innehållet, samt vilka byggstenar, kompromisser eller brister som behöver diskuteras innan kursen påbörjas.

# RANDOM HELP
Some basic Git commands are:
```
git status
git add
git commit
```

Fetching your own repository through composer : 
```
composer config.repositories.namn vcs git@github.com:<username>/repo
composer require <vendor>/<module>
``` 
där vendor/module är som det står i den composer.json

# USEFUL LINKS 
 https://firebearstudio.com/blog/magento-2-developers-cookbook-useful-code-snippets-tips-notes.html
https://magento.stackexchange.com/a/164345
https://devdocs.magento.com/videos/fundamentals/
https://www.safaribooksonline.com/register/
https://www.safaribooksonline.com/search/?query=Mastering%20Magento%202
https://firebearstudio.com/blog/the-best-magento-2-presentations.html

#For windows
https://magento.stackexchange.com/questions/64802/magento-2-404-error-for-scripts-and-css

# RANDOM LINKS FOR MORE READING 
Webpagetest — ( “/lighthouse” för pwa rapport, “/“ ) - för att mäta frontend performance - definieras som allt efter första backendresponse. AMP: 
	
PWA : 
	Preload, Serverpush, Prefetch, Worklets, service workers, http2 = google chrome developers 

Nätverk :  	http2  bra magento vendors, mageplaza, firebear, DivanteLtd, excess 
https://devdocs.magento.com/videos/  L


Todo . 
	installera 2.3 - vue.js /  lazy loading, image optimization  

https://alanstorm.com/category/magento/#magento-for-php-developers   System för magneto I produktion : https://smhttp-nex.nexcesscdn.net/803313/static/vten/white-paper/Nexcess-Magento2-Whitepaper-online_v2.pdf

Preload : https://w3c.github.io/preload/

Prefetch 
 : https://developer.mozilla.org/en-US/docs/Web/HTTP/Link_prefetching_FAQ
 : https://www.w3.org/TR/resource-hints/#dfn-prefetch
 : https://www.chromium.org/spdy/link-headers-and-server-hint

Caching checklist : https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching

Http2  	: https://github.com/bagder/http2-explained/blob/master/sv/part1.md
	: http://varnish-cache.org/docs/trunk/phk/http20.html
	: https://www.upwork.com/hiring/development/the-http2-protocol-its-pros-cons-and-how-to-start-using-it/
 Developer roadmap :  	: https://github.com/kamranahmedse/developer-roadmap
 Magento architecture : 
	: https://devdocs.magento.com/guides/v2.0/architecture/archi_perspectives/arch_diagrams.html Git :   : https://github.com/k88hudson/git-flight-rules/
 GDPR guide : 
	: https://www.mageplaza.com/blog/gdpr-comprehensive-step-by-step-guideline.html 	: https://firebearstudio.com/blog/magento-2-gdpr-compliance-guide.html#What_is_consent_and_how_to_get_it

  docker överallt :  	https://blog.codeship.com/cross-platform-docker-development-environment/
	https://blog.zenika.com/2014/10/07/setting-up-a-development-environment-using-docker-and-vagrant/

Varför ?  	https://medium.com/@cabot_solutions/ansible-and-docker-the-best-combination-for-an-efficient-software-product-management-28c86cfebe90

Working remotely locally :  	file:///Users/nwt/Downloads/NWT-working-remote-locally.webm

Varnish CDN ( fastly )  	: https://www.fastly.com/blog/optimizing-http2-server-push-fastly
