# Sigme Administrator Manual

# Introduction
Welcome to the Sigme Administrator  Manual.
This manual is base on the Xibo Administration Manual and added Japanese translation.

This repository contains the source content Sigme Administrator manual in Japaness. 
The original source contents come from  https://xibosignage.com/docs/setup/
Some modifications and customizations are added at template/custom/sigme directory.

## Support
Prease track issues for Sigme Administrator Manual here: https://github.com/SigmeSignage/sigme-administrator-manual/issues

## Building
The manual is built by running generate.php.
To create sigme Administrator  manual, the build command is

```
php generate.php sigme
```

### Docker
It is also possible to build the manual using Docker, resulting in a Docker
image which hosts the complete manual and a web server.

To do this issue the command:

```
./build.sh -t default -r xibo-manual
```

Where `-t` is your theme name and `-r` is the name with which to tag the 
container.

Themes must exist in `/template/custom/<theme_name>` to be built.
They are build using inheritance from the default theme.
Sigme theme is located this template directory.
