Fedora based image for Jekyll (optionally with github-pages)
------------------------------------------------------------

This project provides a Podman container image based on the Fedora image, usable
for local testing of Jekyll web-pages/blogs.

Optionally your pages can depend on **github-pages** package.  But note that you
can not specify `github-pages` inside your `Gemfile` as `gem 'github-pages'`.
This wouldn't work - the variant installed by bundler from `Gemfile` wouldn't
work with on the Fedora container base where is Ruby >= 3.0.  The absence of
this gem is not a problem in practice because (a) GitHub installs it
automatically, and (b) this projects has it too.


How to use
----------

Simple start can be done out-of-tree:

```
$ JEKYLL_ROOT=<your jekyll directory> make
```

or (if you don't want to clone this repo):

```
$ podman run --rm -ti -p 4000:4000 -v <your/jekyll/root/dir>:/the-jekyll-root:z github-jekyll
```

Though, if you add this project as your git submodule, and you can test anytime
using just the 'make' command:

```
$ cd <your jekyll pages root>
$ git submodule add --name testing-container https://github.com/praiskup/github-pages-fedora-container testing-container
Cloning into '/<your jekyll pages root>/testing-container'...
remote: Enumerating objects: 23, done.
remote: Counting objects: 100% (23/23), done.
remote: Compressing objects: 100% (16/16), done.
remote: Total 23 (delta 7), reused 23 (delta 7), pack-reused 0
Receiving objects: 100% (23/23), 14.77 KiB | 260.00 KiB/s, done.
Resolving deltas: 100% (7/7), done.
$ cd testing-container
$ make
jekyll_root=${JEKYLL_ROOT-`pwd`/..} ; \
rm -f "$jekyll_root"/Gemfile.lock
rm -f ../Gemfile.lock
jekyll_root=${JEKYLL_ROOT-`pwd`/..} ; \
podman run --rm -ti -p 4000:4000 -v $jekyll_root:/the-jekyll-root:z quay.io/praiskup/github-pages
Trying to pull quay.io/praiskup/github-pages:latest...
Getting image source signatures
Copying blob 811daed8c43d done
Copying blob 8b5277f289d7 done
Copying blob 4e287b4fb565 done
Copying blob af8b147315fd done
Copying blob 8905970c141a done
Copying blob 15bfff3f5327 done
Copying config 1ee809b9e8 done
Writing manifest to image destination
Storing signatures
+ bundler exec jekyll serve --drafts --port 5000 --detach
Configuration file: /the-jekyll-root/_config.yml
            Source: /the-jekyll-root
       Destination: /the-jekyll-root/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
       Jekyll Feed: Generating feed for posts
                    done in 0.396 seconds.
 Auto-regeneration: disabled when running server detached.
    Server address: http://127.0.0.1:5000
Server detached with pid '3'. Run `pkill -f jekyll' or `kill -9 3' to stop the server.
+ tinyproxy -d
```

Quit by `CTRL+C`.


Building the image locally?
---------------------------

See above, but just do `make run-local`.
