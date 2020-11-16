# How to create a site like this

This site was create using [docsify](https://docsify.js.org/#/). Layout can be modified following [these instructions](https://jhildenbiddle.github.io/docsify-themeable/#/customization).
The nice thing of docsify is that you can work on the website offline.
[Quick offline](http://localhost:3000/#/)

## Install Node.js
Download and install [Node.js](https://nodejs.org/en/). Node.js contains [nmp](https://www.npmjs.com/get-npm). You need nmp installed to run the following steps.

## Install docsify stuff
It is recommended to install `docsify-cli` globally, which helps initializing and previewing the website locally.

```bash
sudo npm i docsify-cli -g
```

## Initialization
If you want to write the documentation in the `./docs` subdirectory, you can use the `init` command.

```bash
docsify init ./docs
```

## Run offline

Run the local server with `docsify serve`. You can preview your site in your browser on `http://localhost:3000`.

```bash
docsify serve docs
```

## Customization
[Modify the layout of your site](https://jhildenbiddle.github.io/docsify-themeable/#/customization)


#### Enphasis
```
!>
?>
```
!> This is an example

[Tabs plug-in](https://jhildenbiddle.github.io/docsify-tabs/#/)

```
<!-- tabs:start -->
#### **Title**
<!-- tabs:end -->
```
## Emojy
> :smiley: [List of Emojys](https://gist.github.com/rxaviers/7360908)
