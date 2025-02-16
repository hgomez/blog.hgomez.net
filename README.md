# blog.hgomez.net

My blog was hosted on Wordpress, then Octopress, then Hutspot.

I'm switching it to [Hugo](https://gohugo.io/), which seems well-supported and has a ton of plugins and themes, and GitHub Pages integration is very simple via GitHub Action.

## Setup

I installed Hugo using the asdf

```
asdf plugin add hugo
asdf install hugo latest
asdf global hugo latest
```

Then I installed extended hugo 

```
hugo version 

hugo v0.142.0-1f746a872442e66b6afd47c8c04ac42dc92cdb6f+extended darwin/amd64 BuildDate=2025-01-22T12:20:52Z VendorInfo=gohugoio
```
Hugo is 0.142.0, install same extended version with asdf

```
asdf install hugo extended_0.142.0
asdf global hugo extended_0.142.0
```

## First site

I used PaperMod, latest version is 7.0, Feb 2023, so I just download tarball

```
mkdir Documents/Perso
hugo new site blog.hgomez.net
cd blog.hgomez.net
git init

# Grab latest PaperMod (latest release 7.0 didn't works as expected)
curl -L https://github.com/adityatelange/hugo-PaperMod/archive/master.zip -Os
cd themes
unzip ../master.zip
mv hugo-PaperModmaster/ PaperMod
cd ..
rm -f master.zip

# Activation PaperMode Theme
echo "theme = 'PaperMode'" >> hugo.toml
```

Start Hugo Server

```
hugo server
...


                   | EN  
-------------------+-----
  Pages            | 10  
  Paginator pages  |  0  
  Non-page files   |  0  
  Static files     |  1  
  Processed images |  0  
  Aliases          |  1  
  Cleaned          |  0  

Built in 38 ms
Environment: "development"
Serving pages from disk
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1) 
Press Ctrl+C to stop
```

## Install GoatCounter support

### Add analytics support 

Edit themes/PaperMod/layouts/_default/baseof.html

```
<head>
    {{- partial "head.html" . }}
    {{ partialCached "analytics.html" . }}
</head>
```

=>

```
<head>
    {{- partial "head.html" . }}
    {{ partialCached "analytics.html" . }}
</head>
```

### Add GoatCounter Javascript

Edit themes/PaperMod/layouts/partials/analytics.html 

Replace xxxxxxxxxxxx by your GoatCounter Id

```
<script data-goatcounter="https://xxxxxxxxxxxx.goatcounter.com/count"
        async src="https://gc.zgo.at/count.js"></script>
```


