# My Simple Custom Blog Software

The pages on this site are generated by my own primitive blog engine. This post summarizes why I wrote my own, what my priorities for it were and how I went about achieving them.

## Why Something Custom?

Originally I wanted to use [Hugo](https://gohugo.io/) for this blog. I started to look for a simple theme that did not involve many dependencies and provided clean output. This quickly frustrated me because the themes were either not minimal at all or I could not get them to work with my version of Hugo. This made me take a step back and think about what I actually want as blog output. I wrote a tiny HTML header, a small test blog article in markdown and did
`cat header.html > post.html && smu post.md >> post.html`. The result looked good and that convinced me that writing a small blog creation script was more rewarding and maybe even faster than configuring something existing the way I like it.

## Requirements

Before jumping deeper into it, I made a list of my basic requirements:

- Low maintenance
- Low barrier to create content (markdown)
- Low requirements on the client (web browser, internet connection, RAM, CPU)
- Shows creation and update timestamps of posts
- RSS feed

## How I Achieved These  Constraints

### Low Maintenance

- Avoid dependencies: only a POSIX shell and a markdown converter are required. Dependencies always increase the maintenance burden.
- Static files: hosting static files is much easier to do reliably than hosting dynamic content

### Low Barrier to Create Content

- Use markdown: when writing markdown I can mostly avoid thinking about the syntax and focus on the content
- Automatically generate timestamps: generating the timestamps from the git history allows me to just create posts without needing any templates or front matter to provide this data. I also can't forget to update these timestamps.

### Low Requirements on the Client

This is mostly solved by not doing anything complicated. Not adding any JS and CSS frameworks keeps the size small. Not setting a font size and not disallowing zooming makes the text readable on a wide variety of devices and by people of bad eye sight. Just using basic HTML and a few lines of CSS allows old or limited browsers and slow computers to display the page easily. I could go on for a long time here, but I'm sure you get the concept.

### RSS Feed

Adding an RSS feed (an [Atom](https://en.wikipedia.org/wiki/Atom_(Web_standard)) feed to be precise) was the most complicated part of the script. But I really love RSS and in my opinion RSS support is an essential part of a blog. I was shocked to learn that nowadays some blog engines need plugins to get RSS support!
Writing something that is nearly a valid Atom feed is pretty easy. Just take the [example Atom feed](https://validator.w3.org/feed/docs/atom.html#sampleFeed) and fill in your own values. But to make it standard compliant and work well with different clients, I also needed to

* Use an absolute feed URL. I usually avoid absolute URLs, as that makes local testing easier.
* Generate good IDs for the posts. This was the hardest part, see [Mark Pilgram's recommendations](http://web.archive.org/web/20110514113830/http://diveintomark.org/archives/2004/05/28/howto-atom-id) to see the challenges in detail.
* Include the post content while escaping the HTML tags

## Results

### The HTML Output

* No JS, no tracking, no custom fonts
* No external CSS
* Minimal styling
* This page weighs just 7K, most of it the text you are reading right

The largest part of the CSS is the styling of the navigation bar. I could have went with a line of text links separated by spaces, but I wanted to have a clearly visible navigation at the top.
Using such a small amount of CSS also removed the need for any CSS preprocessors like [Sass](https://sass-lang.com/). 

### The Blog Script

You can find the [code of the resulting script](https://github.com/karlb/karl.berlin/blob/master/blog.sh) on github. The blog you are currently viewing is part of the same repository and serves as example content.

To make the script work as intended, you should adhere to the following rules:
- Use git and don't edit history after publishing. This is required for the automatic timestamp generation.
- All posts and the `index.md` have a title using the `# ` syntax. This title is used for the HTMl title tag and for the post titles in the article listing and RSS feed.
- `header.html` contains an absolute link to your `atom.xml`, including a hostname you control. This is necessary to build a correct atom feed.
- Uncommitted files are drafts and won't show up in the normal article list, but are shown in `index-with-drafts.html`.

Feel free to contact me if you have any questions. If you want to use it for your own site, I can give some guidance. Since I don't expect many users, this is less effort than writing and maintaining good documentation.