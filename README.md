# Flip's Set Up Notes

These are notes on how I modified the May 2018 version of `Hugo Theme Academic` (George Cushen) to create my personal website. 

## Main Goals

* Update the `Academic` theme style to include my Feynman diagram footer, which requires some hacks to the style sheets. 

* Note: modifying the footer ends up being a little subtle because of the way the footer margins and padding are tied to the font size. The font size changes (responsive design), which means the footer parameters change with screen size. In turn, this makes it difficult to place graphical footer elements.

## My work environment

I used **netlify** to kickstart my fork of Hugo Theme Academic. This should make it easier to have my university url point to the netlify repository.

I am using the **GitHub** OS X interface to take care of synchronizing with GitHub. I use `Atom` to edit files.

I have a separate folder `\Graphic Sources`` that includes source files for Affinity Designer graphics. 

## 1. Initial Set Up

Here are the steps that I'm taking to go from the clean example site to one that is ready for hacking.

Delete:

* `\content\home\*` *except* `about.md`, `contact.md`, and `teaching.md` (template)
 
* `\content\post` , `\content\publication`, `\content\talk`

* Remove the fluff in `config.toml` ... there's a lot fo commentary. They're good to have handy, but it's distracting when the `config` file gets longer. I'll make a backup and then streamline.


## License

*Copied from original Academic Kickstart `README.md`*

Copyright 2017 [George Cushen](https://georgecushen.com).

Released under the [MIT](https://github.com/sourcethemes/academic-kickstart/blob/master/LICENSE.md) license.

[![Analytics](https://ga-beacon.appspot.com/UA-78646709-2/academic-kickstart/readme?pixel)](https://github.com/igrigorik/ga-beacon)
