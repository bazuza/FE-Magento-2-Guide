# FE Magento 2 Guide 

## Create a storefront theme: walkthrough
The high-level steps required to add a new theme in the Magento system are the following:

### 1. Create a theme directory
To create the directory for your theme:
1. Go to `<your Magento install dir>/app/design/frontend`.
2. Create a new directory named according to your vendor name: `/app/design/frontend/<Vendor>`.
3. Under the vendor directory, create a directory named according to your `theme`. 
```
app/design/frontend/
├── <Vendor>/
│   │   ├──...<theme>/
│   │   │   ├── ...
│   │   │   ├── …
```

### 2. Declare your theme
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

### 3. Make your theme a Composer package (if need)
Magento default themes are distributed as [Composer](https://getcomposer.org/) packages. 
To distribute your theme as a package, add a `composer.json` file to the theme directory and register the package on a packaging server.

Example of a theme `composer.json`:
```
{
    "name": "<Vendore>/<theme>",
    "description": "N/A",
    "require": {
        "php": "~5.5.0|~5.6.0|~7.0.0",
        "magento/theme-frontend-blank": "100.0.*",
        "magento/framework": "100.0.*"
    },
    "type": "magento2-theme",
    "version": "100.0.1",
    "license": [
        "OSL-3.0",
        "AFL-3.0"
    ],
    "autoload": {
        "files": [
            "registration.php"
        ]
    }
}
```

### 4. Add `registration.php`
To register your theme in the system, in your theme directory add a registration.php file with the following content:
```
<?php
/**
 * Copyright © 2015 Magento. All rights reserved.
 * See COPYING.txt for license details.
 */
\Magento\Framework\Component\ComponentRegistrar::register(
    \Magento\Framework\Component\ComponentRegistrar::THEME,
    'frontend/<Vendor>/<theme>',
    __DIR__
);
```

### 5. Create directories for CSS, JavaScript, images, and fonts.

#### Configure images
Product image sizes and other properties used on the storefront are configured in a view.xml configuration file. It is required for a theme, but is optional if exists in the parent theme.
If the product image sizes of your theme differ from those of the parent theme, or if your theme does not inherit from any theme, add view.xml

1. Create the `etc` directory in your theme folder
2. Copy `view.xml` from the etc directory of an existing theme to your theme’s `etc` directory.
```
<?xml version="1.0"?>
<view xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:noNamespaceSchemaLocation="urn:magento:framework:Config/etc/view.xsd">
    <media>
        <images module="Magento_Catalog">
        <image id="category_page_grid" type="small_image">
            <width>200</width>
            <height>200</height>
        </image>
    </media>
</view>
```

#### Create directories for static files
Your theme will likely contain several types of static files: styles, fonts, JavaScript and images. Each type should be stored in a separate sub-directory of `web` in your theme folder:
```
app/design/<area>/<Vendor>/<theme>/
├── web/
│ ├── css/
│ │ ├── source/ 
│ ├── fonts/
│ ├── images/
│ ├── js/
```

#### Your theme directory structure now
At this point your theme file structure looks as follows:
```
app/design/frontend/<Vendor>/
├── <theme>/
│   ├── etc/
│   │   ├── view.xml
│   ├── media/
│   │   ├── preview.jpg
│   ├── web/
│   │   ├── css/
│   │   │   ├── source/ 
│   │   ├── fonts/
│   │   ├── images
│   │   │   ├── logo.svg
│   │   ├── js/
│   ├── registration.php
│   ├── theme.xml
│   ├── composer.json
```

:exclamation: _Copy the less files from the parent theme and paste into your theme. And create in you theme `Magento_Theme => web => css => source => _module.less` directories and less file and copy styles for parent theme_

### 6. Theme registration
Once you open the Magento Admin (or reload any Magento Admin page) having added the theme files to the files system, 
your theme gets registered and added to the database. 

`Content => Configuration => Edit Theme Name (Main Website Store) => select your_theme`

## Compile LESS with Grunt

### Installing and configuring Grunt
1. Install [node.js](https://nodejs.org/en/) to any location on your machine.
2. Install Grunt CLI tool globally. To do this, run the following command in a command prompt: `npm install -g grunt-cli`
3. Install (or refresh) the `node.js` project dependency, including Grunt, for your Magento instance. To do this, run the following commands in a command prompt:
```
cd <your_Magento_instance_directory>
npm install
npm update
```
4. Add your theme to Grunt configuration. To do this, in the dev/tools/grunt/configs/themes.js file, add your theme to module.exports like following:
```
module.exports = {
    ...
    <theme>: {
        area: 'frontend',
        name: '<Vendor>/<theme>',
        locale: '<language>', 
        files: [
            'css/styles-m',
            'css/styles-l'
        ],
        dsl: 'less'
    ...
    },
```
5. (Optional) If you want to use Grunt for "watching" changes automatically, without reloading pages in a browser each time, install the [LiveReload extension](http://livereload.com/extensions/) in your browser.

### Grunt commands:
* `grunt clean:<theme>`
* `grunt exec:<theme>`
* `grunt less:<theme>`
* `grunt watch`

## Compile LESS with Gulp
<table>
<tr><th></th><th>Gulp</th><th>Grunt</th></tr>
<tr><td>Compilation of all themes (10 files):</td><td>16sec</td><td>33sec</td></tr>
<tr><td>Custom theme compilation (2 files)</td><td>4.5s</td><td>11.2s</td></tr>
</table>

### Installation
1. Download as a zip file or clone [this](https://github.com/subodha/magento-2-gulp).
2. Copy `gulpfile.js` and `package.json` in to the root directory
3. Install gulp globaly using `npm install -g gulp-cli`
4. Install modules: run a command in a root directory of your project `npm install`.
5. Compilation: `gulp less --<theme> --live(with live reload)`
6. Watcher: `gulp watch --luma --live(with live reload)`
7. For using [liveReload](http://livereload.com/) install extension for your browser

:exclamation: _Don't remove grunt modules (You must use `gunt exec:<theme>` command for republishes symlinks to the source files)_
