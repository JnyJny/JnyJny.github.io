title: Using Pelican to Publish Your Blog on GitHub
slug: 2018-10-05-GitHub-pelican-blog
category: HowTo
tags: python, blog, pelican, GitHub
date: 2018-10-05
summary: How to use pelican to publish a blog on GitHub.
author: ejo

## If You've Got a GitHub Account, You Can Have a Blog!

[GitHub.com][1] is a hugely popular source code control web service
that uses [git][2] to synchronize local files with copies kept on
GitHub's servers. This lets you easily share and back up your work.
For the purposes of this article I'll assume that you have a GitHub
account, are comfortable with basic git commands and want a blog to
call your own using Pelican.

In addition to it's repository user interface, GitHub also enables
users to [publish web pages][3] of their own directly from a
repository. The web site generation package that GitHub recommends is
[Jekyll][4], written in Ruby. Since I'm a bigger fan of [Python][5], I
went with [Pelican][6] instead.

I'll describe how to install Pelican, set up your GitHub repository
and publish your first article. 

### The Basics

Pelican and Jekyll both transform content written in [Markdown][9] or
[ReStructured Text][8] into HTML and generate a static web site. Both
generators support themes which allow for infinite amounts of
customization. We'll install pelican, run a quick start helper, write
some Markdown files and then publish our website to GitHub.

Easy peasy.

### Installing Pelican & Creating the Repo

First things first, you need pelican (and ghp-import) installed on
your local machine.  This is super easy with the pip, the python package
installation tool (you have pip right?):

```
$ pip install pelican ghp-import
```

Next, open up a browser and create a new repository on GitHub for your
new sweet blog. Name it as follows (use your GitHub user name):

```
https://GitHub.com/username/username.github.io
```

and leave it empty, we will fill it up with compelling blog content in
a second.

Using a command-line (you command-line right?), clone your empty git
repository to your local machine:

```bash
$ git clone https://GitHub.com/username/username.github.io blog
$ cd blog
```

### That One Weird Trick...

Now here's the trick with publishing web content on GitHub which isn't
super obvious. For user pages, pages hosted in repos named
_username.github.io_, the content is served from the `master`
branch. But we don't want to publish all the pelican configuration
files and whatnot to `master`, just the web content. So we keep the
pelican configuration and the raw content in a seperate branch that I
like to call 'content'. You can call it whatever you want, but I'll
just assume you called it content too.

```bash
$ git checkout -b content
Switched to a new branch 'content'
```
### Configuring Pelican

Now, here comes the content configuration. Pelican provides a great
initialization tool called 'pelican-quickstart' that will ask you a
series of questions about your blog.

```bash
$ pelican-quickstart
Welcome to pelican-quickstart v3.7.1.

This script will help you create a new Pelican-based website.

Please answer the following questions so this script can generate the files
needed by Pelican.

> Where do you want to create your new web site? [.]  
> What will be the title of this web site? Super blog
> Who will be the author of this web site? username
> What will be the default language of this web site? [en] 
> Do you want to specify a URL prefix? e.g., http://example.com   (Y/n) n
> Do you want to enable article pagination? (Y/n) 
> How many articles per page do you want? [10] 
> What is your time zone? [Europe/Paris] US/Central
> Do you want to generate a Fabfile/Makefile to automate generation and publishing? (Y/n) y
> Do you want an auto-reload & simpleHTTP script to assist with theme and site development? (Y/n) y
> Do you want to upload your website using FTP? (y/N) n
> Do you want to upload your website using SSH? (y/N) n
> Do you want to upload your website using Dropbox? (y/N) n
> Do you want to upload your website using S3? (y/N) n
> Do you want to upload your website using Rackspace Cloud Files? (y/N) n
> Do you want to upload your website using GitHub Pages? (y/N) y
> Is this your personal page (username.github.io)? (y/N) y
Done. Your new project is available at /Users/username/blog
```

The only questions I didn't take the defaults on were:

* web site title
* web site author
* time zone
* upload to GitHub

After answering all the questions, pelican leaves the following
offerings in the current directory:

```bash
$ ls
Makefile		content/	develop_server.sh*
fabfile.py		output/		pelicanconf.py
publishconf.py
```

You can go checkout the [Pelican docs][10] to find out how to use
all of those files, but we're all about getting things down *right now*.
No, I haven't read the docs yet either.

### Forging On

```bash
$ git add .
$ git commit -m 'initial pelican commit to content'
$ git push origin content
```

So here we've added all the pelican generated files to the `content`
branch of the local git repo, commited the changes and then pushed the
local changes to the remote repo hosted on GitHub. Not super exciting,
but it will be handy if we need to revert edits to one of these files.

### Finally Getting Somewhere

Ok, now we get bloggy! All of your blog posts, photos, images, pdfs,
etc will live in the `content` directory which is initially
empty. I'll talk you thru creating a first post and an 'About' page
with a photo.

```bash
$ cd content
$ mkdir pages images
$ cp /Users/username/SecretStash/HotPhotoOfMe.jpg images
$ touch first-post.md
$ touch pages/about.md

```

Next open the empty file `first-post.md` in your favorite editor
and add the following text:

```markdown
title: First Post on My Sweet New Blog
date: <today's date>
author: Your Name Here

#I am On My Way To Internet Fame and Fortune!

This is my first post on my new blog. While not super informative it
should convey my sense of excitement and eagerness to engage with you,
the reader!
```

The first three lines are metadata that pelican uses to organize things. There
are lots of different metadata you can put there, again the docs are your best
bet for learning more about them.

Now we'll open up the empty file `pages/about.md` and add this text:

```markdown
title: About
date: <today's date>

![So Schmexy][my_sweet_photo]

Hi, I am <username> and I wrote this epic collection of Interweb
wisdom. In days of yore much of this would have been deemed sorcery
and I would probably have been burned at the stake.

😆

[my_sweet_photo]: {filename}/images/HotPhotoOfMe.jpg
```

So now we have three new pieces of web content in our content
directory.  Of the content branch. That's a lot of content.

### Publishing 

Don't worry, the pay off is coming!

All that's left to do is:

* Run pelican to generate the static HTML files in `output`:

```bash
   $ pelican content -o output -s publishconf.py
```

* Use `ghp-import` to add the contents of the output directory to the `master` branch:

```bash
   $ ghp-import -m "Generate Pelican site" --no-jekyll -b master output
```

* Push the local master branch to the remote repo:

```bash
   $ git push origin master
```

* Commit and push the new content to the 'content' branch:

```bash
   $ git add content
   $ git commit -m 'added a first post, a photo and an about page'
   $ git push origin content
```

### OMG I Did It!

Now the exciting part is here, when you get to see what you've
published for everyone to see! Type into your browser the URL:

```
https://username.github.io
```

Congratulations on your new blog, self-published on GitHub!


[1]: https://github.com/
[2]: https://git-scm.com
[3]: https://help.github.com/categories/GitHub-pages-basics/
[4]: https://jekyllrb.com
[5]: https://python.org
[6]: https://blog.getpelican.com
[7]: https://www.pelicanthemes.com
[8]: http://docutils.sourceforge.net/docs/user/rst/quickref.html
[9]: https://guides.github.com/features/mastering-markdown
[10]: https://docs.getpelican.com
