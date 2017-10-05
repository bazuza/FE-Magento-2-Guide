#Create Custom Magento Theme

##Create a storefront theme: walkthrough
The high-level steps required to add a new theme in the Magento system are the following:
1. Create a directory for the theme under `app/design/frontend/<your_vendor_name>/<your_theme_name>`.
2. Add a declaration file `theme.xml` and optionally create etc directory and create a file named `view.xml` to the theme directory.
3. Add a `composer.json file`.
4. Add registration.php.
5. Create directories for CSS, JavaScript, images, and fonts.
6. Configure your theme in the Admin panel.

##Create a theme directory
To create the directory for your theme:
1. Go to `<your Magento install dir>/app/design/frontend`.
2. Create a new directory named according to your vendor name: `/app/design/frontend/<Vendor>`.
3. Under the vendor directory, create a directory named according to your theme. 
```
app/design/frontend/
├── <Vendor>/
│   │   ├──...<theme>/
│   │   │   ├── ...
│   │   │   ├── …
```

##Declare your theme
After you create a directory for your theme, you must create `theme.xml` containing at least the theme name and the parent theme name (if the theme inherits from one). Optionally you can specify where the theme preview image is stored.
1. Add or copy from an existing `theme.xml` to your theme directory `app/design/frontend/<Vendor>/<theme>`
2. Configure it using the following example:

```
<theme xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/theme.xsd">
     <title>New theme</title> <!-- your theme's name -->
     <parent>Magento/blank</parent> <!-- the parent theme, in case your theme inherits from an existing theme -->
     <media>
         <preview_image>media/preview.jpg</preview_image> <!-- the path to your theme's preview image -->
     </media>
</theme>
```

##Make your theme a Composer package (if need)
Magento default themes are distributed as Composer packages. 
To distribute your theme as a package, add a `composer.json` file to the theme directory and register the package on a packaging server. 
A default public packaging server is [https://packagist.org/](https://packagist.org/). `composer.json` provides theme dependency information.






