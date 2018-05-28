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


## 2. Footer and Main Page Design

The biggest change is to update the footer. Since this is the part that's been giving me the most headaches.

1. Create a `<div id="THECONTENT">`. The purpose: this will be a white canvas. The `<body>` tag behind it will be dark gray to match the header and footer so that "overscrolling" will not look strange. (A) Make folder `/layouts/partials/` to include template overwrites. (B) Copy the following files from the corresponding `template` folder `footer_container.html`, `head_custom.html`, and `navbar.html`. 

2. Add the following line to the bottom of `navbar.html`: 

```html
<div id="THECONTENT"> <!-- closed in footer_container.html; see flip2019.css -->
```

3. Add the following line to the bottom of `footer_container.html`:

```html
</div> <!--  ends id=#THECONTENT from navbar.html; see flip2019.css -->
```

4. Add folder: `/static/css/` Insert file `flip2019.css` (based on `flip2018.css`) This introduces a bunch of kludges. For now, the main point of this file is to contain:

```css
footer {
  /* font-family: 'Raleway', sans-serif; */
  background-color:#333333;
}

#botbar1{
	background-color:#C1BBAB;
	margin: 0;
	padding: 0;
  position:relative;
  top: 85px;
	left: 0px;
	z-index: 9;
	height: 7px;
	width: 100%;
}

body{
  background-color:#333333;
}
```

5. Load `flip2019layout.css` by updating the `custom_css` line in `config.toml`:

```
custom_css = ['flip2019layout.css']
```

At this point, you should have a dark gray footer. Now let's get to the gray line separating the main content and the footer. This is what we call `#botbar1`. 

6. In `\layouts\partials\footer_container.html`, insert a `#botbar` div:

```html
<footer class="site-footer">
  <!-- FLIP'S NEW STUFF BELOW -->
  <div id="botbar1"></div>
  <!-- FLIP'S NEW STUFF ABOVE -->
  <div class="container">

```

The bottom line should show up at the bottom of the page. You should play with the `#botbar1` top margin in `flip2019layout.css` to see where this line ends up. There's something pressing and subtle to sort out first, though:

7. Fix the `footer` style so that it's written in `px` rather than `rem` or `em`. The problem with `rem` (relative to root) or `em` is that when the font size changes from responsive design, the padding will shift which will force you to re-calculate the top shift of `#botbar`.


```css
footer {
  /* font-family: 'Raleway', sans-serif; */
  background-color:#333333;
  /* Fixing academic.css line 979  */
  margin: 64px 0 0;
  padding: 32px 0;
}

#botbar1{
	background-color:#C1BBAB;
	margin: 0;
	padding: 0;
  position:relative;
  top: -35px;
	left: 0px;
	z-index: 9;
	height: 7px;
	width: 100%;
}
```

8. Now insert Feynman footer. Insert iamge into `/static/img/layout/feymmanfooter.png`. Here's what seems to work for `flip2019layout.css`:

```css
#feynmanfoot{
	background-size: 250px 200px;
	background-repeat:no-repeat;
	background-position:left top;
  pointer-events: none;
	margin: 0;
	padding: 0;
  position: relative;
  /* bottom: -100px;  */ /* if you place div above footer */
  bottom: 205px;
  left: 40vw;
	z-index: 200;
	height: 200px;
	width: 250px;
	text-align: right;
}
```

And here's what I put into `footer_container.html`:

```html
<footer class="site-footer">
  <!-- FLIP'S NEW STUFF BELOW -->
  <div id="botbar1"> </div>
  <div style="position: relative; width: 0; height: 0">
    <div id="feynmanfoot" style="background-image:url('{{ .Site.BaseURL }}/img/{{ .Site.Params.footmark }}');"></div>
  </div>
  <div style="height: 20px">
    <!-- JUST A SPACER -->
  </div>
  <!-- FLIP'S NEW STUFF ABOVE -->
  <div class="container">
```

(Note: there's an error in the 2018 version of my styel file where a stray `$` makes the `feymanfoot` div fail to be found on Netlify.)

## 3. Personalization

1. `config.toml`: `avatar = "profile/Flip600_sq.jpg"` (and upload the respective file)

2. In `header.html` (theme default) we have the following: So insert `icon.png` and `icon-192.png` as appropriate.
<!--add `\static\favicon.ico` In -->

```html
  <link rel="icon" type="image/png" href="{{ "/img/icon.png" | relURL }}">
  <link rel="apple-touch-icon" type="image/png" href="{{ "/img/icon-192.png" | relURL }}">
```

(You don't need to make edits to `header.html` for this.)

3. `cryptedemail.css` is a hack for displaying e-mail addresses in a way that isn't too easy for bots to scrape. Include it in the `config.toml` list of css files. It goes with `\layouts\shortcodes\flipemail.html` which provides a simple way to insert e-mail addresses. You'll also want to break up the e-mail address in `config.toml`:

```
  # email = "test@example.org"  # using cryptedmail
  email1 = "flip.tanedo" # using cryptedmail
  email2 = "ucr" # using cryptedmail
  email3 = "edu" # using cryptedmail
```

Commenting out the old e-mail removes that line in the `contact` widget. We'll use the [cryptedmail trick](https://stackoverflow.com/a/41566570). Create a `contact.html` widget in `\layouts\partials\widgets\` to 

```html
<!-- FLIP UPDATE -->
<!-- from: https://stackoverflow.com/a/41566570 -->
      
      {{ with $.Site.Params.email1 }}
      <li>
        <i class="fa-li fa fa-envelope fa-2x" aria-hidden="true"></i>
        <span id="person-email" itemprop="email">
        <!-- {{- if $autolink }}<a href="mailto:{{ . }}">{{ $.Site.Params.email2 }}</a>{{ else }}{{ . }}{{ end -}} -->
          <a data-name="{{ $.Site.Params.email1 }}" data-domain="{{ $.Site.Params.email2 }}" data-tld="{{ $.Site.Params.email3 }}" href="#" class="cryptedmail" onclick="window.location.href = 'mailto:' + this.dataset.name + '@' + this.dataset.domain + '.' + this.dataset.tld"></a>
        </span>
      </li>
      {{ end }}

<!-- /FLIP UPDATE -->
```

4. Added `flip2018.css` (introduces `comment` class, also Google calendar settings), and `flipaboutme.css` which modifies the `about` widget.

5. Added `about2.html` in `\layouts\partials\widgets` and modified `\content\home\about.md` to have the line

```md
widget = "about2"
```

## 4. Fonts 'n stuff

1. I shouldn't need to install any fonts in `head_custom.html`; do *not* do:

```html
<link href="https://fonts.googleapis.com/css?family=Titillium+Web" rel="stylesheet">
```

2. I'm in the middle of loading `flipfont`

## License

*Copied from original Academic Kickstart `README.md`*

Copyright 2017 [George Cushen](https://georgecushen.com).

Released under the [MIT](https://github.com/sourcethemes/academic-kickstart/blob/master/LICENSE.md) license.

[![Analytics](https://ga-beacon.appspot.com/UA-78646709-2/academic-kickstart/readme?pixel)](https://github.com/igrigorik/ga-beacon)
