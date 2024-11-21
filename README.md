# www.simongilbert.net V2 (Hugo)
This blog is based on the theme which resides in another of your repositories - [https://github.com/sahgilbert/hugo-blog-awesome](https://github.com/sahgilbert/hugo-blog-awesome).

In order to style the current repository, you need to make changes to the theme repository, and then pull them down as a new git submodule, into this repository.

### Create New Hugo Site
```
$ hugo new site name-of-my-site-goes-here
$ cd name-of-my-site-goes-here
$ git init
$ git submodule add https://github.com/sahgilbert/hugo-blog-awesome.git themes/hugo-blog-awesome
$ echo "theme = 'hugo-blog-awesome'" >> hugo.toml
$ hugo server
```