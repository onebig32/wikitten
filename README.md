---
title: Wikitten and MDwiki merged Markdown Wiki system
author: Uranus Zhou
---

# Wikitten and MDwiki merged Markdown Wiki system

## Introduction

This Wikitten and MDwiki merged Wiki system is based on PHP Wiki [Wikitten](http://wikitten.vizuina.com/) and pure static JavaScript Wiki [MDwiki](http://www.mdwiki.info/).

You can deploy this merged Wiki system on your PHP web server (via Wikitten) or static web server, also you can view your Wiki locally (via MDwiki).

I made some modifications to MDwiki and Wikitten, so they can share the same Wiki document directory and show similar Wiki interface.

### Changes to MDwiki

* Put all Markdown document to a new `library` directory, which is the same to Wikitten;
* Support [YAML front matter](http://jekyllrb.com/docs/frontmatter/) at the beginning of Markdown file;
 
 > **Note**
 > 
 > YAML front matter support is already in MDwiki develop version, it should comes to final user in further release.
 > YAML front matter in Markdown document should begin and end with three back-ticks (`` ` ``) instead of three dashes (-).
 
* I write an auto generate index files script **`generate-index.sh`** for MDwiki `library` directory, run this in Bash shell:
 
 ```bash
 ./generate-index.sh library
 ```

 Then **`generate-index.sh`** will generate `index.md` for all sub-directories (only if one directory doesn't contain valid `index.md` document).

 Once you add new Markdown document or delete/rename document in `library` directory, you should run this **`generate-index.sh`** to generate new index files.

* Support full content searching Markdown document by modified version **lunr.js**, I write an auto generate search index file node.js program **`search.js`**, MDwiki and Wikitten use **search_index.json** index file in Wiki program directory.

 ```bash
 node search.js library search_index.json
 ```

 If you need full content searching support, you should run this **`search.js`** after making any changes to Wiki library.

### Changes to Wikitten

* Support YAML front matter which begin and end with both three back-ticks (`` ` ``) and three dashes (-);
* Show `index.md` document if switch to on directory (instead of tell user to select in Wiki library tree);
* Set Markdown table width to 100%;
* Support full content searching Markdown document, use **`search.js`** node.js program to generate **search_index.json** search index file.

## Deploy

Note: Wikitten and MDwiki share the same `library` Markdown document directory.

### Deploy Wikitten

Please refer to [Wikitten](http://wikitten.vizuina.com/) website, you need at least PHP 5.3 and php_fileinfo module, and your web server should support rewrite.

Wikitten config file is `config.php` in root directory.

 > **Note**
 > 
 > You need to change Apache or Nginx rule for **search_index.json** if you need full content searching support, or it will be served by Wikitten PHP by default.
 > 
 > Nginx rule may like this:
 > 
 > ```
 > location ~* ^/search_index.json$ {
 >     access_log off;
 >     expires max;
 > }
 > ```

### Deploy MDwiki

Please refer to [MDwiki](http://www.mdwiki.info/) website.

MDwiki config file is `config.json` in root directory, I add a new `parseHeader` config in `config.json` file (this is already in MDwiki develop version).

## Contact

Any problems? Feel free to add new issue or contact me at [https://zohead.com/](https://zohead.com/).
