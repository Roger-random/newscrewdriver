---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
This is a text mirror of my WordPress site
[https://newscrewdriver.com](https://newscrewdriver.com)
, a snapshot as of end of year 2024. It was an experiment to see the tradeoffs
involved if I should migrate New Screwdriver from WordPress.com to Jekyll on
GitHub Pages. Jekyll project that generated this page can be seen at its
[GitHub Repository](https://github.com/Roger-random/newscrewdriver)

Observations:

* GitHub Pages has a size limit of 1GB, and I have almost that much in image
files. While I could in theory move the whole thing over today, I would not be
able to continue for very long before I run into the limit. For now I decided
not to migrate the images: all image URLs still point back to WordPress.

* Speaking of images, the "Featured Image" of each post is kind of pain. It is
listed on a post under `meta:` as `_thumbnail_id:` which then has to be matched
up against `post_id` in an attachment XML file. I don't see a simple way
to port that over to Jekyll. Some code will need to be written. Without such
code, posts listed below have lost their featured images.

* Speaking of `post_id`, that was one of the pieces of metadata NOT processed
from the WordPress.com exported XML file by the standard
[Jekyll WordPress.com importer](https://import.jekyllrb.com/docs/wordpressdotcom/)
so I had to modify the code to also pick up `post_id` to make featured image
lookup possible with the aforementioned code. (Look in the GitHub repository
for file `jekyll-import.importers.wordpressdotcom.modified.rb`)

* The exporter had a function `--no-fetch-images` but I couldn't figure out
how to use it. No matter what I do it always seems to try to fetch images into
the `assets` folder. I already had a copy of those images on hand, but more
importantly, I didn't want to sit through over 800MB of downloads!. After
failing to figure out how to use the parameter as intended, I went with the
blunt instrument approach and disabled that functionality entirely in my
aforementioned modified exporter.

* The exporter brought over `/_posts/` as expected, and all images had an
associated descriptor file (with `post_id`, etc.) in `/_attachments/`. Those
I understood. The exporter also brought over other files I deleted as I didn't
think I need them for this snapshot:
    * `/_drafts/` I don't need to publish my WordPress drafts
    * `/_nav_menu_items/` I am using Jekyll-generation navigation now.
    * `/_pages/` I had just two pages. One is outdated and other an accident.
    * `/_wp_global_styless/` I assume this has something to do with WordPress
    style sheet but there was nothing I recognized as CSS. In any case I'm
    using Jekyll theme styles now.

* The exporter failed to un-escape some encoded text. For example a recent
post title is "Tiny Cat &amp;amp; Galactic Squid" when it should have been un-
escaped to "Tiny Cat & Galactic Squid". It also fails to escape encode symbols
that were fine as-is in WordPress but needed to be modifed for Liquid. For
example posts about Angular framework uses curly-brace encodings that have
special meaning for Liquid templating engine used in Jekyll. Fixing them in an
automated fashion (I sure as heck ain't doing it manually) will be an annoying
task with many edge cases.

* Another task I want to automate: change all absolute links to the base
url newscrewdriver.com to relative links. Thanks to those absolute links,
ever URL here points back to the currently live site, making it more difficult
to experiment independently from the live site or to build my own locally
hosted mirror at home.

* I thought I would need some kind of pagination solution because I have over
2400 posts on WordPress. But I tried it anyway so the flat list below has
all of them. Surprisingly, modern web browsers seem to handle this extremely
long list just fine as long as I have just the headline and avoid heavier
features like images or text excerpts. Good enough for me to not worry about
pagination for now. It does take a while to generate (~26 seconds on my Jekyll
playground VM) but that may be reasonable for the number of posts. It also
makes me more interested in the advertised speed of [Hugo](https://gohugo.io/).
Maybe that'll be another experiment for later.

* On the performance front: Jekyll seems to run single-threaded and use only
one core. This is inefficient in today's multicore world. I'm curious if Hugo
(written in multicore-friendly Go) will perform better on this front.

* I will lose the commenting system if I switch. Historically commenting
traffic has been low so it wouldn't be a huge loss.

* I will lose text search if I switch. Since NewScrewdriver.com has become my
project notebook, I frequently reference back to my notes. Losing text search
would make it much more difficult to find my past notes. I'm much more
reluctant to make this tradeoff.
