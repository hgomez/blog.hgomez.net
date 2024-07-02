+++
title = 'New Blog using Hugo'
date = 2024-06-24T13:20:23+02:00
draft = false
tags = [ 'SRE', 'Talk' ]
+++

A first, my blog was hosted on Wordpress.

There was so many attacks attempts on the wordpress powered sites I worked on that, I deviced to move to something simple, [Octopress](http://octopress.org/).

Octopress incidently turned End Of Life in 2015, so I had to move to something else, [Hubpress](https://github.com/HubPress) who was stopped in 2019.

And for a few years, blog was waiting for a new engine, here enter [Hugo](https://gohugo.io/), a well-supported engine, with a lot of plugins and themes, and whereGitHub Pages integration is very simple via GitHub Action.  

## Install Hugo

I installed Hugo using the [asdf](https://asdf-vm.com/)

```
asdf plugin add hugo
asdf install hugo latest
asdf global hugo latest
```

Then I installed extended hugo

```
hugo version

hugo v0.128.0-e6d2712ee062321dc2fc49e963597dd5a6157660+extended linux/amd64 BuildDate=2024-06-25T16:15:48Z VendorInfo=gohugoio
```

Hugo being 0.128.0, install same extended version with asdf

```
asdf install hugo extended_0.128.0
asdf global hugo extended_0.128.0
```

## Blog Theme - PaperMode

There is really a lot of themes with Hugo, I decided to go with something simple, [PaperMod](https://github.com/adityatelange/hugo-PaperMod)
I tried the latest released version, 7.0, Feb 2023, but it didn't worked as expected, so I moved to the latest version in master (something I always try to avoid)
  

```
mkdir Documents/Perso
hugo new site blog.hgomez.net
cd blog.hgomez.net
git init

# Grab latest PaperMod (get master as latest release 7.0 didn't works as expected)
curl -L https://github.com/adityatelange/hugo-PaperMod/archive/master.zip -Os
cd themes
unzip ../master.zip
mv hugo-PaperMod-master/ PaperMod
cd ..
# Housekeeping :)
rm -f master.zip
rm -f hugo.toml 
```
Default configuration, in yaml file

```title=hugo.yaml
baseURL: https://blog.hgomez.net/
languageCode: en-us
title: Rico's Blog
theme: ["PaperMod"]

enableEmoji: true

params:

  defaultTheme: auto

  homeInfoParams:
    Title: Welcome to Rico's Blog
    Content: OSS activist, ASF Member, former Tomcat contributor, founder of JPackage, DevOps Incubator and OBuildFactory projects. Dev, QA and Ops. 

  socialIcons: 
    - name: "X"
      url: "https://x.com/hgomez"
    - name: "GitHub"
      url: "https://github.com/hgomez/"
    - name: "LinkedIn"
      url: "https://fr.linkedin.com/in/gomezhe"

  ShowShareButtons: false
  ShowReadingTime: true
  ShowPostNavLinks: true
  ShowCodeCopyButtons: true

outputs:
  home:
    - HTML
    - RSS
    - JSON
EOF
```
## Let's have a look

```
hugo server
...

| EN
-------------------+-----
Pages | 10
Paginator pages | 0
Non-page files | 0
Static files | 1
Processed images | 0
Aliases | 1
Cleaned | 0

Built in 38 ms
Environment: "development"
Serving pages from disk
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

![first blog](blog0.png)

Quite promising so far 😉

We'll see next time how bring contents, expose blog in GitHub pages and get a free tracking without GoogleAnalytics.