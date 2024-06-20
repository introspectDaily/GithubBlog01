<<<<<<< HEAD
# The Hacker-Blog theme

*Hacker-Blog is a minimalistic, responsive jekyll theme built for hackers. It is based on the [hacker theme](https://github.com/pages-themes/hacker) for project pages.*

Demo: [https://ashishchaudhary.in/hacker-blog](https://ashishchaudhary.in/hacker-blog)

### Included

1. Pagination
2. SEO tags
3. Archive Page
4. About Page
5. RSS (`https://base-url/atom`)
6. Sitemap (`https://base-url/sitemap`)
7. Google Analytics (optional)

## Usage

1. Fork and Clone this repository
2. Customize your blog
3. Add a new post in `_posts/` directory with proper name format (as shown in placeholder posts)
4. Commit and push to master on a repository named `<githubusername.github.io>`.
5. Visit `<githubusername>.github.io`

## Local Build

If you want to see the changes before pushing the blog to Github, do a local build.

1. [`gem install jekyll`](https://jekyllrb.com/docs/installation/#install-with-rubygems)
2. `gem install jekyll-seo-tag`
3. `gem install jekyll-paginate`
4. `gem install jekyll-sitemap`
5. (`cd` to the blog directory, then:) `jekyll serve --watch --port 8000`
6. Go to `http://0.0.0.0:8000/` in your web browser.

*Note: In case you have set a `baseurl` different than `/` in `_config.yml`, go to `http://0.0.0.0:8000/BASEURL/` instead.*

### Local build using docker

```bash
docker run --rm -p 8000:8000 \
  --volume="LOCATION_OF_YOUR_JEKYLL_BLOG:/srv/jekyll" \
  -it tocttou/jekyll:3.5 \
  jekyll serve --watch --port 8000
```

Replace `LOCATION_OF_YOUR_JEKYLL_BLOG` with the full path of your blog repository. Visit `http://localhost:8000/` to access the blog.

*Note: In case you have set a `baseurl` different than `/` in `_config.yml`, go to `http://0.0.0.0:8000/BASEURL/` instead.*

## Customizing

### Configuration variables

Edit the `_config.yml` file and set the following variables:

```yml
title: [The title of your blog]
description: [A short description of your blog's purpose]
author:
  name: [Your name]
  email: [Your email address]
  url: [URL of your website]

baseurl: [The base url for this blog.]

paginate: [Number of posts in one paginated section (default: 3)]
owner: [Your name]
year: [Current Year]
```

*Note: All links in the site are prepended with `baseurl`. Default `baseurl` is `/`. Any other baseurl can be setup like `baseurl: /hacker-blog`, which makes the site available at `http://domain.name/hacker-blog`.*

Additionally, you may choose to set the following optional variables:

```yml
google_analytics: [Your Google Analytics tracking ID]
```

### About Page

Edit `about.md`

### Layout

If you would like to modify the site style:

**HTML**

Footer: Edit `_includes/footer.html`

Header: Edit `_includes/header.html`

Links in the header: Edit `_includes/links.html`

Meta tags, blog title display, and additional CSS: Edit `_includes/head.html`

Index page layout: Edit `_layouts/default.html`

Post layout: Edit `_layouts/post.html`

**CSS**

Site wide CSS: Edit `_sass/base.scss`

Custom CSS: Make `_sass/custom.scss` and use it. Then add `@import "custom";` to `css/main.scss`

**404 page**

Edit `404.md`

## License

CC0 1.0 Universal
=======
<!-- markdownlint-disable-next-line -->
<div align="center">

  <!-- markdownlint-disable-next-line -->
  # Chirpy Jekyll Theme

  A minimal, responsive, and feature-rich Jekyll theme for technical writing.

  [![Open in Dev Containers](https://img.shields.io/static/v1?label=Dev%20Containers&logo=visualstudiocode&message=Open&color=deepskyblue)][open-container]&nbsp;
  [![Gem Version](https://img.shields.io/gem/v/jekyll-theme-chirpy?&logo=RubyGems&logoColor=ghostwhite&label=gem&color=tomato)][gem]&nbsp;
  [![GitHub license](https://img.shields.io/github/license/cotes2020/jekyll-theme-chirpy)][license]&nbsp;
  [![CI](https://img.shields.io/github/actions/workflow/status/cotes2020/jekyll-theme-chirpy/ci.yml?logo=github)][ci]&nbsp;
  [![Codacy Badge](https://app.codacy.com/project/badge/Grade/4e556876a3c54d5e8f2d2857c4f43894)][codacy]

  [**Live Demo** →][demo]

  [![Devices Mockup](https://chirpy-img.netlify.app/commons/devices-mockup.png)][demo]

</div>

## Features

- Dark Theme
- Localized UI language
- Pinned Posts on Home Page
- Hierarchical Categories
- Trending Tags
- Table of Contents
- Last Modified Date
- Syntax Highlighting
- Mathematical Expressions
- Mermaid Diagrams & Flowcharts
- Dark Mode Images
- Embed Media
- Comment Systems
- Built-in Search
- Atom Feeds
- PWA
- Web Analytics
- SEO & Performance Optimization

## Documentation

To learn how to use, develop, and upgrade the project, please refer to the [Wiki][wiki].

## Contributing

Contributions (_pull requests_, _issues_, and _discussions_) are what make the open-source community such an amazing place
to learn, inspire, and create. Any contributions you make are greatly appreciated.
For details, see the "[Contributing Guidelines][contribute-guide]".

## Credits

### Contributors

Thanks to [all the contributors][contributors] involved in the development of the project!

[![all-contributors](https://contrib.rocks/image?repo=cotes2020/jekyll-theme-chirpy&columns=16)][contributors]
<sub> — Made with [contrib.rocks](https://contrib.rocks)</sub>

### Third-Party Assets

This project is built on the [Jekyll][jekyllrb] ecosystem and some [great libraries][lib], and is developed using [VS Code][vscode] as well as tools provided by [JetBrains][jetbrains] under a non-commercial open-source software license.

The avatar and favicon for the project's website are from [ClipartMAX][clipartmax].

## License

This project is published under [MIT License][license].

[gem]: https://rubygems.org/gems/jekyll-theme-chirpy
[ci]: https://github.com/cotes2020/jekyll-theme-chirpy/actions/workflows/ci.yml?query=event%3Apush+branch%3Amaster
[codacy]: https://app.codacy.com/gh/cotes2020/jekyll-theme-chirpy/dashboard?utm_source=gh&utm_medium=referral&utm_content=&utm_campaign=Badge_grade
[license]: https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/LICENSE
[open-container]: https://vscode.dev/redirect?url=vscode://ms-vscode-remote.remote-containers/cloneInVolume?url=https://github.com/cotes2020/jekyll-theme-chirpy
[jekyllrb]: https://jekyllrb.com/
[clipartmax]: https://www.clipartmax.com/middle/m2i8b1m2K9Z5m2K9_ant-clipart-childrens-ant-cute/
[demo]: https://cotes2020.github.io/chirpy-demo/
[wiki]: https://github.com/cotes2020/jekyll-theme-chirpy/wiki
[contribute-guide]: https://github.com/cotes2020/jekyll-theme-chirpy/blob/master/docs/CONTRIBUTING.md
[contributors]: https://github.com/cotes2020/jekyll-theme-chirpy/graphs/contributors
[lib]: https://github.com/cotes2020/chirpy-static-assets
[vscode]: https://code.visualstudio.com/
[jetbrains]: https://www.jetbrains.com/?from=jekyll-theme-chirpy
>>>>>>> my-pages
