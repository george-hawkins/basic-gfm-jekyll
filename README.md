# Serve Markdown via github.io

GitHub Pages are great but wouldn't you like to be able to use `.md` files instead of having to provide HTML?

As it turns out with the right boilerplate this is simple to do and is demonstrated here.

Compare the file `redcarpet-extensions.md` as it appears here on github.com and as it appears served as HTML via github.io:

* [github.com redcarpet-extensions.md](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/redcarpet-extensions.md)
* [github.io redcarpet-extensions.html](https://george-hawkins.github.io/basic-gfm-jekyll/redcarpet-extensions.html)

Note the change in suffix between the two - from `.md` to `.html`.

If you compare the two you will notice some slight differences:

* When looked at on github.com you'll see an initial small table (showing layout as "default" and title as "Configuring Redcarpet") that isn't present in the github.io version. This is front matter and is explained later.
* If you go down to the [highlight section on github.io](https://george-hawkins.github.io/basic-gfm-jekyll/redcarpet-extensions.html#highlight) and compare it with the same [section on github.com](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/redcarpet-extensions.md#highlight) you'll see highlighting in action on the github.io version but not on the github.com version. This is because Github filters out all but a subset of HTML tags when your content is displayed mixed in with theirs.
* The content area is slightly wider on github.io than on github.com - this is because the content area has expanded out into the area that was taken up by the GitHub sidebar for settings etc.
* The syntax highlighting is slightly different - the github.io version uses the standard settings of a highlighter called [Pygments](http://pygments.org/docs/quickstart/) while the version on github.com uses GitHub's own setup.

The important thing is front matter - it's the one change you have to make to a standard `.md` file in order for it to be served via github.io.

This `README.md` will not served via github.io as it has no front matter.

If you look at the [raw version of redcarpet-extensions.md](https://raw.githubusercontent.com/george-hawkins/basic-gfm-jekyll/gh-pages/redcarpet-extensions.md) you'll see an initial block of text:

    ---
    layout: default
    title: Configuring Redcarpet
    ---

This is the [front matter](http://jekyllrb.com/docs/frontmatter/) - several variables between lines of three minuses. Any number of variables can be specified - the block above shows the simplest case - a layout name ("default" which corresponds to a `.html` file covered later) and a title for the resulting HTML page.

## Creating a copy

To use this repository as a basis for your own you need to create an empty repository, let's call it "my-pages", on GitHub and then duplicate "basic-gfm-jekyll" like so:

```bash
$ git clone --bare git@github.com:george-hawkins/basic-gfm-jekyll.git
$ cd basic-gfm-jekyll.git  
$ git push --mirror git@github.com:<your-username>/my-pages
$ rm -rf basic-gfm-jekyll.git
```

Replace `<your-username>` with your GitHub username. Once you've duplicated the repository you can now pull down your copy:

```bash
$ git clone git@github.com:<your-username>/my-pages
$ cd my-pages
```

Duplicating repositories is covered in more detail on [GitHub help](https://help.github.com/articles/duplicating-a-repository/).

If you aren't [using an ssh key](https://help.github.com/articles/generating-ssh-keys/#step-3-add-your-ssh-key-to-your-account) with your account you'll have to use `https` style URLS rather than the `git` style ones used above, i.e. replace `git@github.com:` with `https://github.com/`.

Now you'll need to make two small but important changes to the file `_config.yml` found in the base directory of the repository.

In `_config.yml` you'll see the following two lines:

```YAML
baseurl: "/basic-gfm-jekyll"
url: "https://george-hawkins.github.io"
```

You need to replace `basic-gfm-jekyll` with the name of your repository, i.e. `my-pages` in this example, and you need to replace `george-hawkins` with your GitHub username.

Without the `baseurl` change nothing will work at all and without the `url` change your github.io content will references my github.io site.

## Creating new content

Now that you have a copy of this repository you can remove `README.md` and `redcarpet-extensions.md`, everything else is necessary boilerplate.

To add new content simply create new `.md` files and remember to add front matter as shown above.

**Important:** without a front matter block a `.md` file will not be served via github.io.

Any new content, once pushed to GitHub, can be accessed via github.io by replacing the `.md` in the file name with `.html`, i.e. `my-content.md` becomes `my-content.html` when retrieved via github.io.

That's it - for some explanation of the mechanics read on.

## Jekyll

GitHub uses a static site generator called [Jekyll](http://jekyllrb.com/) that converts various formats, such as `.md`, into HTML and also supports a templating engine called [Liquid](https://github.com/Shopify/liquid/wiki/Liquid-for-Designers).

`_config.xml` etc. along with front matter drive how the Jekyll engine will render `.md` files into the HTML content served via github.io.

Jekyll supports various Markdown processors, the one that handles `.md` in the way that GitHub does is called [Redcarpet](https://github.com/vmg/redcarpet/).

## Boilerplate

So what are the various boilerplate files? Here's a brief explanation of each:

* [`_config.yml`](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/_config.yml) - this contains variables and configuration for Jekyll, see the [Jekyll configuration page](http://jekyllrb.com/docs/configuration/) for more details.
* [`_includes/anchor_links.html`](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/_includes/anchor_links.html) - this create the visible anchors you see when you hover your mouse over a heading, see the "[hover anchors](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/redcarpet-extensions.md#hover-anchors)" section of `redcarpet-extensions.md` for more details.
* [`_layouts/default.html`](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/_layouts/default.html) - take a look at this file, it provides the very simply HTML that surrounds your `.md` content (which ends up where you see `{{ content }}`). The name of this file minus the `.html` suffix, i.e. `default`, corresponds to the layout name you saw above in the front matter.
* [`css/container.css`](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/css/container.css) - this is the very simple CSS for the two `<div>` elements seen in `default.html`.
* [`css/syntax.css`](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/css/syntax.css) - this is the CSS needed for syntax highlighting.

As mentioned above syntax highlighting is handled by a tool called Pygments and the `syntax.css` file was generated using the `pygmentize` tool like so:

```bash
$ pygmentize -S default -f html > css/syntax.css
```

In addition to the two `.css` files mentioned `default.html` pulls in two further `.css` files that replicate the `.css` that GitHub uses when serving `.md` content:

* `github-markdown.css` - this comes from Sindre Sorhus's [github-markdown-css](https://github.com/sindresorhus/github-markdown-css) project and provides the CSS needed for the GitHub Markdown style.
* `normalize.css` - this comes from Nicolas Gallagher's [normalize.css](http://necolas.github.io/normalize.css/) project and tries to make all HTML elements render more consistently across all browsers.

## Final notes

The page `redcarpet-extensions.md` is mainly intended as an example of making `.md` content available via github.io but it does also explain the various options you can specify in `_config.yml` that control how Redcarpet works.

You can use Liquid markup in your pages served via github.io, however this markup is not processed when the same `.md` is viewed on github.com. For an example see how the Liquid tag `raw` is not processed when you look at the ["hover anchors" section](https://github.com/george-hawkins/basic-gfm-jekyll/blob/gh-pages/redcarpet-extensions.md#hover-anchors) of `redcarpet-extensions.md` on github.com but is consumed when you look at the same [section on github.io](https://george-hawkins.github.io/basic-gfm-jekyll/redcarpet-extensions.html#hover-anchors).
