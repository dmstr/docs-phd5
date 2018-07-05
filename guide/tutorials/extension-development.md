Extension Development
=====================

If you plan to reuse functionality, it is recommended to provide the code in composer packages. Luckily, Yii 2.0 Framework is equipped with a code generator for creating ready to use extension skeletons in a minute.

Building an extension
---------------------

> Note! Code generation is only available with `YII_ENV_DEV=true`.

### Generating skeleton with Gii

To create an extension from the command line adjust the parameters in the following command and run it from your
project root.

```
 ./yii gii/extension \
     --vendorName=mycompany \
     --packageName=yii2-mypackage \
     --namespace=mycompany\\mypackage\\ \
     --license=BSD-3-Clause \
     --title="Extension for Yii 2.0 Framework" \
     --description="Provides cool stuff" \
     --authorName="Your Name" \
     --authorEmail=you@yourdomain.com
```

This will generate a folder in `runtime/tmp-extensions/yii2-mypackage` with example code for the extension.

### Make your extension available via composer

Create a new private or public repository, eg. on [GitHub](https://github.com/new).

Then, go to the folder with the generated code inside your project's `runtime` folder and initializie a temporary
Git repository

```
cd runtime/tmp-extensions/yii2-mypackage
git init
git add .
```

Check if you have added the correct files with

```
git status
```

It should show three files to be committed. If everything is fine commit and push your changes to your repo.

```
git commit -m "initial commit"
git remote add origin https://github.com/mycompany/yii2-mypackage.git
git push origin master
```

This is a one-time push from this repo, you should install your extension via composer now.

#### Private packages

If your package should be availible via [Toran Proxy](https://toran.hrzg.de) add repository URL to [repository list](https://toran.hrzg.de/repositories/) and add a Deploy Key to project settings.

If you are developing packages for a private repository you can enable your package by adding it's repository URL to `composer.json`

```
"repositories": [
{
  "type": "git",
  "url": "git@github.com:mycompany/yii2-mypackage.git"
},
```

> You should use the git protocol when using private repositories in conjunction with private-public-key authentication when
> deploying remotely or in a virtualized environment. You can add `container-files` like `.ssh/known_hosts` or `.gitconfig` in the 
> `Dockerfile`

#### Public packages

To make your extension available publicly, [submit it to packagist](https://packagist.org/packages/submit).
 
It should be available after a few minutes, if you are heavily developing an extension you can still add it's repository 
manually to `composer.json`, like described above. Composer will then check the repository on every update and you'll
get always the latest commits.

### Install package

Now get the code in any Yii2 application with

```
composer require mycompany/yii2-mypackage:dev-master
```

Depending on your extension and its bootstrapping configuration you may have to configure it in the application configuration.
The classes from your package are available via auto-loading and accessible by their namespace. 

### Alternative: Using the Gii Web interface

You can also use the Gii web-interface and your favorite Git UI client to accomplish the tasks described above.

- Open `Gii > Extension Generator`
- Enter values
- Click preview
- Click generate
- Create Git repo, commit and push to repository
- Require via `composer`


Forking 3rd-party extensions in vendor
--------------------------------------

Set constraint to `@dev` before starting code modifications and run 
    
    composer require vendor/package:@dev
     
> If you've already changed code in a dist-package, you can move away the package with the changes.
> `mv package _package`, run the above command and then copy only the contents from the modified folder
> into the package folder. `git status` should now your changes now.

Update configuration to get sources for the forked package

    "config": {
        "preferred-install": {
            "the-vendor/*": "source",
            "*": "auto"
        },
    },

### Forking asset repositories

TBD

See also https://github.com/dmstr-forks
