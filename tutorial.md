Websites in RStudio Tutorial
================
Dani Cosme & Sam Chavez
6/2/2018

-   [Create a website in RStudio](#create-a-website-in-rstudio)
    -   [1. Background info](#background-info)
    -   [Why use RStudio to make static websites?](#why-use-rstudio-to-make-static-websites)
    -   [What is blogdown?](#what-is-blogdown)
    -   [What is Hugo?](#what-is-hugo)
-   [2. Learn the basics](#learn-the-basics)
    -   [install blogdown and hugo](#install-blogdown-and-hugo)
    -   [update hugo if necessary](#update-hugo-if-necessary)
    -   [create a website using the default lithium template](#create-a-website-using-the-default-lithium-template)
    -   [render the site](#render-the-site)
    -   [where does the content live?](#where-does-the-content-live)
    -   [config.toml file](#config.toml-file)
    -   [about page](#about-page)
    -   [posts](#posts)
        -   [Create a new post](#create-a-new-post)
            -   [Markdown post](#markdown-post)
    -   [add r code chunk](#add-r-code-chunk)
    -   [plot something](#plot-something)
        -   [Rmarkdown post](#rmarkdown-post)
    -   [create academic website using the `hugo-academic` template by gcushed](#create-academic-website-using-the-hugo-academic-template-by-gcushed)
-   [render the site](#render-the-site-1)

Create a website in RStudio
===========================

Overview of the tutorial 1. Background into 2. Learn the basics 3. Create an academic website 4. Publish your website to GitHub 5. Explore advanced features

1. Background info
------------------

Why use RStudio to make static websites?
----------------------------------------

What is blogdown?
-----------------

What is Hugo?
-------------

2. Learn the basics
===================

install blogdown and hugo
-------------------------

    install.packages("blogdown")
    library(blogdown)
    blogdown::install_hugo()

update hugo if necessary
------------------------

    blogdown::hugo_version() 
    blogdown::update_hugo()

create a website using the default lithium template
---------------------------------------------------

    blogdown::new_site()

render the site
---------------

    blogdown::serve_site()

where does the content live?
----------------------------

Key components: + config.toml = configuration file that the user edits to customize their site + index.Rmd = + content/ = where the actual content (e.g. pages, posts live) + public/ =

config.toml file
----------------

`[[menu.main]]` =

-   Can be external links (e.g. Twitter page) or internal links (e.g. /posts/)
-   Default arrangement is alphabetical order

Add the following text to your `config.toml` page:

    [[menu.main]]
        name = "CV"
        url = "https://twitter.com/rstudio"
    [[menu.main]]
        name = "Zed"
        url = "https://twitter.com/rstudio"

about page
----------

1.  Open `content/about/md` and check it out.
2.  Add the following text to the file and look at the difference.

<!-- -->

    We're writing in markdown here. Some useful syntax.

    ## h2 header
    ### h3 header
    #### h4 header

    **bold**<br>
    *italics*<br>
    ~~strikethrough~~

    Check out [Cory's awesome markdown tutorial](https://github.com/uodatascience/markdown) for more markdown magic.

    Also not that you can write in html as well.

    <img src="https://media.giphy.com/media/l0Nwvo3slpo6nS0PC/giphy.gif" alt="neato">

posts
-----

1.  Open `content/post` and look around

Add the following text to your `config.toml` page:

    [[menu.main]]
        name = "Posts"
        url = "/post/"

### Create a new post

The `new_post()` function will automatically create a new post and append the date to the front of the file name that you specify as an input.

#### Markdown post

    blogdown::new_post("newmd", ext = '.md')

Add the following text to the new `.md` file and view it in the browser:

Here is some text. Lots of text. So much text.

**Gee this is fun!**

Let's add a table, just for kicks.

| hours of sun | happiness |
|--------------|-----------|
| 0            | 1         |
| 3            | 4         |
| 5            | 7         |
| 7            | 10        |

add r code chunk
----------------

``` r
mean(iris$Sepal.Length)
```

    ## [1] 5.843333

plot something
--------------

``` r
require(ggplot2)
```

    ## Loading required package: ggplot2

``` r
ggplot(iris, aes(Sepal.Length, Sepal.Width, color = Species)) +
  geom_point() +
  geom_smooth(method = "lm") + 
  scale_color_manual(values = c("#3B9AB2", "#E4B80E", "#F21A00"))
```

![](tutorial_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-2-1.png)

#### Rmarkdown post

    blogdown::new_post("newrmd", ext = '.Rmd')

Add the same text as above to your `.Rmd` file and view it in the browser.

create academic website using the `hugo-academic` template by gcushed
---------------------------------------------------------------------

    gcushen/hugo-academic

render the site
===============

Addins &gt; Serve Site

    blogdown::serve_site()
