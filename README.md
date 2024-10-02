# Toku

[![github pages](https://github.com/b4ae67c9beb8/tokumei-webapp/actions/workflows/gh-pages.yml/badge.svg)](https://github.com/b4ae67c9beb8/tokumei-webapp/actions/workflows/gh-pages.yml)
[![GNU General Public License v3.0 license](https://img.shields.io/github/license/b4ae67c9beb8/tokumei-webapp)](https://github.com/b4ae67c9beb8/tokumei-webapp/blob/main/LICENSE)

## Overview

This is a personal blog powered by the lightweight [Bear Cub](https://github.com/clente/hugo-bearcub) theme, a Hugo theme designed for speed and optimization. With a focus on delivering high-quality content without unnecessary distractions, it offers a clean and responsive design for a seamless reading experience.

## Kudos to Bear Cub

I want to give a special shoutout to the creators of **Bear Cub**. This theme has made it incredibly easy to create a fast and accessible blog. Its features, such as multilingual support, automatic RSS feed generation are essential for anyone looking to share their thoughts with the world.

You can check out the Bear Cub theme here: [Bear Cub GitHub Repository](https://github.com/clente/hugo-bearcub).

## Installation

Follow Hugo's [quick start](https://gohugo.io/getting-started/quick-start/) to create an empty website. To install the Bear Cub theme, clone it into your themes directory as a Git submodule:

```sh
    git submodule add https://github.com/clente/hugo-bearcub themes/hugo-bearcub
```

Then, append the following line to your site configuration file:

```sh
    echo 'theme = "hugo-bearcub"' >> hugo.toml
```

## Features
    - Responsive Design: Looks great on any device.
    - Lightweight: Optimized for fast loading times (~5kb per page).
    - No Trackers or Ads: Focus on content without distractions.
    - Automatic RSS Feed: Keep your readers updated with new posts.
    - Accessibility: Enhanced for users with visual and motor impairments.
    - Multilingual Support: Write in multiple languages effortlessly.

## Configuration

You can customize Toku using the hugo.toml file. Here’s an example configuration:

```toml

baseURL = "https://your-domain.com"
theme = "hugo-bearcub"
copyright = "Your Name (CC BY 4.0)"
defaultContentLanguage = "en"

[params]
  title = "Toku"
  description = "Welcome to my blog where I share my thoughts and experiences."
  favicon = "images/favicon.png"
```

## Contributing

If you find any issues or would like to contribute to this page, feel free to open an issue or create a pull request. Your contributions are welcome!

## License

This project is licensed under the MIT License - see the [LICENSE](https://github.com/b4ae67c9beb8/tokumei-webapp/blob/main/LICENSE) file for details.