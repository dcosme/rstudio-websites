Websites in RStudio Tutorial
================
Dani Cosme & Sam Chavez
06-03-2018

-   [Overview of the tutorial](#overview-of-the-tutorial)
-   [Background](#background)
    -   [Why use RStudio to make static websites?](#why-use-rstudio-to-make-static-websites)
    -   [What is blogdown?](#what-is-blogdown)
    -   [What is Hugo?](#what-is-hugo)
-   [Learn the basics](#learn-the-basics)
    -   [Install blogdown and hugo](#install-blogdown-and-hugo)
    -   [Update hugo if necessary](#update-hugo-if-necessary)
    -   [Create a website using the default lithium template](#create-a-website-using-the-default-lithium-template)
    -   [Render the site](#render-the-site)
    -   [Where does the content live?](#where-does-the-content-live)
    -   [`config.toml` file](#config.toml-file)
    -   [Homepage](#homepage)
    -   [About page](#about-page)
    -   [Pages](#pages)
    -   [Posts](#posts)
-   [Advanced features](#advanced-features)
    -   [Password protection](#password-protection)
    -   [Shiny apps](#shiny-apps)
    -   [Commenting](#commenting)
    -   [Custom CSS (?)](#custom-css)
-   [Create academic website using the `hugo-academic` template](#create-academic-website-using-the-hugo-academic-template)
-   [Publish your website](#publish-your-website)
-   [Workflow](#workflow)
-   [Helpful resources](#helpful-resources)
-   [Mini (mega? ulimate?) hack](#mini-mega-ulimate-hack)

### Overview of the tutorial

1.  Background
2.  Learn the basics
3.  Create an academic website
4.  Explore advanced features
5.  Publish your website to GitHub

Background
==========

Most of the information in this tutorial is coming from a few amazing resources on using the *blogdown* package to create static websites in RStudio using R Markdown:

-   [blogdown: Creating Websites with R Markdown](https://bookdown.org/yihui/blogdown/)
-   Various presentations and posts by Alison Hill
    -   <https://apreshill.github.io/data-vis-labs-2018/slides/06-slides_blogdown.html#1>
    -   <https://alison.rbind.io/slides/blogdown-workshop-slides>
    -   <https://alison.rbind.io/post/up-and-running-with-blogdown/#build-your-site-in-rstudio>

Why use RStudio to make static websites?
----------------------------------------

-   It's easy
-   It's free
-   It's a simple way to share your code and analyses

What is blogdown?
-----------------

What is Hugo?
-------------

Learn the basics
================

Install blogdown and hugo
-------------------------

    install.packages("blogdown")
    library(blogdown)
    blogdown::install_hugo()

Update hugo if necessary
------------------------

    blogdown::hugo_version() 
    blogdown::update_hugo()

Create a website using the default lithium template
---------------------------------------------------

There are a number of ways to create a new website project. You can create it using the IDE by clikcing `File > New Project > New Directory > Website Using Blogdown` or my simply specifying the path to the new website as a argument in the `blogdown::new_site()` function. Make sure that you specify the new directory (e.g. the name of the folder you would like to create) in the path.

``` r
blogdown::new_site("default-site")
```

You can also specify a theme when creating your new site. The default is the [lithium theme](https://github.com/yihui/hugo-lithium), but there are a variety of other [Hugo themes](https://themes.gohugo.io/) available. We'll use the [hugo-academic](https://github.com/gcushen/hugo-academic) theme later in the tutorial.

    blogdown::new_site(theme = 'yihui/hugo-lithium')
    blogdown::new_site(theme = 'devcows/hugo-universal-theme')
    blogdown::new_site(theme = 'gcushen/hugo-academic')

Render the site
---------------

Anytime you want to render the site, you can do so by either clicking `Addins > Serve Site` or executing the `blogdown::serve_site()` function. This function should be executed from the site directory, so if you are not currently in that directory, make sure to change you current working directory to the site directory.

``` r
setwd("default-site/")
blogdown::serve_site()
```

Where does the content live?
----------------------------

Take a look at what's in the site directory.

Key components:
+ `config.toml` = configuration file that the user edits to customize their site
+ `content/` = where the actual content (e.g. pages, posts live)
+ `public/` = the directory containing the website for online deployment

`config.toml` file
------------------

The configuration file is where \[X\]. More about configuration files and the variables specified within it [here](http://gohugo.io/getting-started/configuration/)

### Edit `config.toml`

Change the site name

    title = "Dani Cosme, M.S."

Enable emojis

    enableEmoji = true

### Navigation bar

More on menu management [here](https://gohugo.io/variables/menus/). `[[menu.main]]` = Pages and links in the menu bar

-   Can be external links (e.g. Twitter page) or internal links (e.g. /posts/)
-   Default arrangement is alphabetical order
-   To manually specify the order, add `weight` as a field

Add the following text to your `config.toml` page and see what happens:

    [[menu.main]]
        name = "UO Data Science Club"
        url = "https://github.com/uodatascience"
        weight = 1

Homepage
--------

Create a file called `index.html` in the `layouts/` directory.

``` bash
touch default-site/layouts/index.html
```

Copy the following text into `layouts/index.html`. I borrowed this code from [Alison Hill's awesome blogdown tutorial](https://github.com/rladies-pdx/rladies-PDX/blob/master/layouts/index.html), but you could write your own html code to create the layout for your homepage.

    {{ partial "header" . }}

    <div class="intro">
            <div><center><img class="center-image" src={{ .Site.Params.img }} width="20%"/></center></div>

            <h1><center>{{ markdownify .Title }}</center></h1>
            
            <h2><center>{{ markdownify .Site.Params.Description }}</center></h2>

    </div>


    {{ partial "footer" . }}

We referenced a field`.Site.Params.img`, but it currently doesn't exist. Let's add it to `config.toml` and also update the site description.

    [params]
        description = "Welcome to my website!<br>Here is a bunch of text about me.<br>Yada yada yada."
        img = "https://avatars1.githubusercontent.com/u/11858670?s=460&v=4"

Since the posts are no longer on linked from the landing page, let's add the `Posts` page to the navigation bar by adding it as a field in `config.toml`:

    [[menu.main]]
        name = "Posts"
        url = "/post/"

About page
----------

Open `/content/about.md` and check it out. Add the following text to the `about.md` file and look at the difference.

    Because we enabled emojis in `config.toml`, we can use them here. I :heart: emojis!

    Posts are written in plain markdown. Here is some useful syntax. More on plain markdown in the default post [A Plain Markdown Post](../2016/12/30/a-plain-markdown-post/). There's also lots more information in the digital book [blogdown: Creating Websites with R Markdown](https://bookdown.org/yihui/blogdown/output-format.html).

    ## h2 header
    ### h3 header
    #### h4 header

    **bold**<br>
    *italics*<br>
    ~~strikethrough~~

    Also check out [Cory's awesome markdown tutorial](https://github.com/uodatascience/markdown) for more markdown magic.

    Note: you can write in html.

    <h4> Embed a gif <br>
    <img src="https://media.giphy.com/media/l0Nwvo3slpo6nS0PC/giphy.gif" alt="neato">

    <h4> Embed a calendar <br>
    <iframe src="https://calendar.google.com/calendar/embed?src=beo4lsbjns0kqovh8nktjou8l4%40group.calendar.google.com&ctz=America%2FLos_Angeles" style="border: 0" width="800" height="600" frameborder="0" scrolling="no"></iframe>

Pages
-----

Create a new page called `CV` in the content folder.

``` r
setwd("default-site/")
blogdown::new_content("content/cv")
```

Copy the following text into the document:

    ---
    title: ''
    date: '2018-06-03 23:25:08 GMT'
    ---

    <div style="text-align: right"><h1>DANI COSME</div>
    <div style="text-align: right">dcosme@uoregon.edu</div>
    <div style="text-align: right"><a href="http://dcosme.github.io">dcosme.github.io</a></div>

    ### EDUCATION //
    **Doctoral Candidate, Psychology, 2015-present**<br>
    University of Oregon (Eugene, OR)<br>
    Advisors: Drs. Jennifer Pfeifer & Elliot Berkman

    ### PUBLICATIONS //
    **Cosme, D.** (2018) Brilliant article. *Science*, 1(1), 1-5.<br>
    [DOI](http://doi.org/) [OSF](http://osf.io/) [NEUROVAULT](http://neurovault.org/)

Add the page to the navigation bar by adding the following text to `config.toml`. Note that while the filepath is `site root/content/cv.md`, the webpath is `/baseurl/cv`.

    [[menu.main]]
        name = "CV"
        url = "/cv/"

This is the start of a super basic mardown CV, but there are a number of more advanced templates out there, such as [this one](https://github.com/elipapa/markdown-cv).

Posts
-----

Open `content/post` and look around.

### Create a new post

Using the IDE: `Addins > New Post`

In the console, the `new_post()` function will automatically create a new post and append the date to the front of the file name that you specify as an input.

#### Markdown post

Let's create a plain markdown post and add some content.

``` r
setwd("default-site/")
blogdown::new_post("newmd", ext = '.md')
```

Add the following text to the new `.md` file:

    Here is some text. Lots of text. So much text.

    **Gee this is fun!**

    Let's add a table, just for kicks.

    | hours of sun | happiness |
    |---|---|
    | 0 | 1 |
    | 3 | 4 |
    | 5 | 7 |
    | 7 | 10 |

Add the following r code chunks (removing `eval=FALSE`) and view the file in the browser:

``` r
mean(iris$Sepal.Length)
```

``` r
require(ggplot2)
ggplot(iris, aes(Sepal.Length, Sepal.Width, color = Species)) +
  geom_point() +
  geom_smooth(method = "lm") + 
  scale_color_manual(values = c("#3B9AB2", "#E4B80E", "#F21A00"))
```

#### Rmarkdown post

Now let's create a plain markdown post and see how it differs from the plain markdown file.

``` r
setwd("default-site/")
blogdown::new_post("newrmd", ext = '.Rmd')
```

Add the same text as above to your `.Rmd` file and view it in the browser. We can see that the text looks similar in both files, but only in the R Markdown file are we able to execute code chunks.

This feature makes R Mardown posts ideal for sharing code via your website.

### Tags and categories

Advanced features
=================

Password protection
-------------------

Shiny apps
----------

Commenting
----------

Custom CSS (?)
--------------

Create academic website using the `hugo-academic` template
==========================================================

Now that we've gotten our bearings with the default template, let's such out a popular template for making personal academic website created by [George Cushen](https://github.com/gcushen/hugo-academic).

Here are a few examples of how people (including Dani) have modified this template: + [Alison Hill](https://alison.rbind.io/) + [Flip Tandeo](http://physics.ucr.edu/~flip/) + [Amlan Kar](https://amlankar.github.io/) + [Yu Cheng](https://fischcheng.github.io/) + [Dani Cosme](https://dcosme.github.io/)

We'll call our new website `academic-site` and it will be saved to the `rstudio-websites` directory.

``` r
blogdown::new_site("academic-site", theme = "gcushen/hugo-academic")
```

Render the site, if necessary.

``` r
setwd("academic-site/")
blogdown::serve_site()
```

Click around the rendered website. What features do you like? What features seem useful? + Tags and categories

Now, let's check out the file structure. + config file + content + publications

Publish your website
====================

To create your website for publishing, execute `blogdown::hugo_build()`. This will create/update the `public/` directory in your site's root directory.

More on the recommended workflow [here](https://bookdown.org/yihui/blogdown/workflow.html).

Workflow
========

Here is the [suggested workflow](https://slides.yihui.name/2017-rmarkdown-UNL-Yihui-Xie.html#30) from the main developer of blogdown, Yihui:

> 1.  Open your website project, click the "Serve Site" addin
> 2.  Revise old pages/posts, or click the "New Post" addin
> 3.  Write and save (take a look at the automatic preview)
> 4.  Push everything to Github, or upload the public/ directory to Netlify directly

Helpful resources
=================

-   <https://bookdown.org/yihui/blogdown/>
-   <https://apreshill.github.io/data-vis-labs-2018/slides/06-slides_blogdown.html#1>
-   <https://alison.rbind.io/slides/blogdown-workshop-slides>
-   <https://alison.rbind.io/post/up-and-running-with-blogdown/#build-your-site-in-rstudio>
-   <http://amber.rbind.io/blog/2016/12/19/creatingsite/>
-   <https://gohugo.io/>
-   <http://elipapa.github.io/markdown-cv/>

Mini (mega? ulimate?) hack
==========================

You final hack in this class if to create a personal website and post it to GitHub. You can use any template you want or make it from scratch using the [*rmarkdown* package](http://nickstrayer.me/RMarkdown_Sites_tutorial/).

Here are the component you should include:

1.  Homepage with a brief description about your research interests
2.  A CV (either written in markdown or simply as a PDF you've uploaded)
3.  Your tutorial from this class as a post

Change colors, futz with custom CSS, make a shiny app and link to it -- feel free to add anything else you'd like!

Go 🍌 🍌!

Or just do the bare minimum because it's the last week of the term 😄
