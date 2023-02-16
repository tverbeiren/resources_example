Minimum reproducible example for issue 4377
================

This is a minimum reproducible example for
[quarto-dev/quarto-cli#4377](https://github.com/quarto-dev/quarto-cli/issues/4377).

First clone this repo.

``` sh
git clone git@github.com:tverbeiren/resources_example.git && cd resources_example
```

## First time execution

The document `document.qmd` uses a template which contains a resources
directory. The first time around, everything works fine.

``` bash
quarto render document.qmd
```
      

    Output created: document.pdf

## Rerunning a second time

This time around, resources exists but cannot be overwritten. Hence, the
render command fails.

``` bash
quarto render document.qmd
```
      
    ERROR: PermissionDenied: Permission denied (os error 13), remove '/home/tverbeir/resources_example/resources/quarto.png'

We can indeed see that the resources directory doesn’t have write
permissions.

``` bash
ls -l
```

    total 48
    drwxr-xr-x. 1 tverbeir tverbeir    16 Feb 16 15:20 document_files
    -rw-r--r--. 1 tverbeir tverbeir 19013 Feb 16 15:20 document.pdf
    -rw-r--r--. 1 tverbeir tverbeir   112 Feb 16 15:06 document.qmd
    -rw-------. 1 tverbeir tverbeir  4925 Feb 16 15:20 document.tex
    drwxr-xr-x. 1 tverbeir tverbeir    34 Feb 16 15:02 _extensions
    -rw-r--r--. 1 tverbeir tverbeir  4651 Feb 16 15:20 README.md
    -rw-r--r--. 1 tverbeir tverbeir  1345 Feb 16 15:20 README.qmd
    -rw-r--r--. 1 tverbeir tverbeir  1356 Feb 16 15:20 README.rmarkdown
    dr-xr-xr-x. 1 tverbeir tverbeir    20 Feb 16 15:02 resources

``` bash
ls -l resources
```

    total 12
    -rw-r--r--. 1 tverbeir tverbeir 11810 Feb 16 15:02 quarto.png

``` bash
ls -l _extensions/resources_example/resources
```

    total 12
    -rw-r--r--. 1 tverbeir tverbeir 11810 Feb 16 15:02 quarto.png

## Third time’s the charm

By making the resources directory writeable again, we can again rerender
the document.

``` bash
chmod +w -R resources
quarto render document.qmd
```

    Output created: document.pdf

Is it possible that `quarto` also performs a `chmod +w` on template
resources, but doesn’t do this recursively?
