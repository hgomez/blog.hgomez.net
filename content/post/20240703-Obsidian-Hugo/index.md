+++
title = 'Obsidian and Hugo'
date = 2024-07-03T03:20:23+02:00
draft = false
tags = [ 'Hugo', 'Obsidian' ]
+++

	
I've been using [Obsidian](https://obsidian.md/) for several months now to take my notes. It's an easy-to-use tool for writing in markdown mode, and it's very simple to insert images or links.

Obsidian organizes documentation into [Vaults](https://help.obsidian.md/Getting+started/Create+a+vault), which are simply folders. This makes it very easy to have the said content in a Git repository and thus have your notes shared on multiple devices, which is very practical when you want to switch from a personal computer to a professional one.

There is paid plans on Obsidian to [sync contents](https://obsidian.md/sync) on your devices and even [publish to the web](https://obsidian.md/publish) but this is not something I'm using right now, using git is enough for me.

[Hugo](https://gohugo.io/) is a blog engine using Markdown, there was obviously something to do to get blog contents produced via Obsidian.

It was quite simple.

1. I created a blog part inside Obsidian vault 
2. I copied old blogs contents by copy/pasting text and images.
3. One article is a directory, using my own convention YYYYMMDD-Title 
4. Inside the folder, an **index.md** and extra contents like images

![Layout](obsidian-1.png)

Obsidian create a titlte.md file, so your title will be **index** and you enter the blog metadata contents as expected by Hugo

![index](obsidian-2.png)
That's all, your document have the structure expected by Hugo.
You can edit your file and when done, copy it to your Hugo directory.

For example :

* Obsidian blog contents is hosted under `$HOME/Documents/Perso/Blog/blog.hgomez.net`
* Hugo blog is hosted under `$HOME/Documents/Perso/blog.hgomez.net`

I only have to copy Obsidian blog contents under `content/post` directory in Hugo

```
cp -rf ~/Documents/Perso/obsidian-hgomez/Blog/blog.hgomez.net/* ~/Documents/Perso/blog.hgomez.net/content/post/
```

With this structure, you have a working copy of your blog in Obsidian with Hugo hosting elsewhere, your Obsidian and Hugo are totaly separated and can have their own lifecycle.

⚠ Hugo won't produce pages where date is set in the future, when editing your date in blog post, be sure to have something set in the past 😇