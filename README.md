# html2md
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-1-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

[![MIT License](http://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![GoDoc](https://godoc.org/github.com/suntong/html2md?status.svg)](http://godoc.org/github.com/suntong/html2md)
[![Go Report Card](https://goreportcard.com/badge/github.com/suntong/html2md)](https://goreportcard.com/report/github.com/suntong/html2md)
[![Build Status](https://github.com/suntong/html2md/actions/workflows/go-release-build.yml/badge.svg?branch=master)](https://github.com/suntong/html2md/actions/workflows/go-release-build.yml)
[![PoweredBy WireFrame](https://github.com/go-easygen/wireframe/blob/master/PoweredBy-WireFrame-B.svg)](http://godoc.org/github.com/go-easygen/wireframe)



## TOC
- [html2md - HTML to Markdown converter](#html2md---html-to-markdown-converter)
- [Usage](#usage)
  - [$ html2md](#-html2md)
  - [Examples](#examples)
    - [Simplest form](#simplest-form)
    - [Using goquery](#using-goquery)
  - [The options and plugins](#the-options-and-plugins)
    - [Testing the new table plugins](#testing-the-new-table-plugins)
  - [Credits](#credits)
    - [Credits](#credits-1)
    - [Similar Projects](#similar-projects)

## html2md - HTML to Markdown converter

The `html2md` makes use of `github.com/JohannesKaufmann/html-to-markdown`
to convert HTML into Markdown, which is using an [HTML Parser](https://github.com/PuerkitoBio/goquery) to avoid the use of `regexp` as much as possible, which can prevent some [weird cases](https://stackoverflow.com/a/1732454) and allows it to be used for cases where the input is totally unknown.

![gopher stading on top of a machine that converts a box of html to blocks of markdown](https://github.com/JohannesKaufmann/html-to-markdown/raw/master/logo.png)


## Usage

### $ html2md
```sh
HTML to Markdown
Version 1.0.0 built on 2023-05-02
Copyright (C) 2020-2023, Tong Sun

HTML to Markdown converter on command line

Usage:
  html2md [Options...]

Options:

  -h, --help                       display help information 
  -i, --in                        *The html/xml file to read from (or stdin) 
  -d, --domain                     Domain of the web page, needed for links when --in is not url 
  -s, --sel                        CSS/goquery selectors [=body]
  -v, --verbose                    Verbose mode (Multiple -v options increase the verbosity.) 

      --opt-heading-style          Option HeadingStyle 
      --opt-horizontal-rule        Option HorizontalRule 
      --opt-bullet-list-marker     Option BulletListMarker 
      --opt-code-block-style       Option CodeBlockStyle 
      --opt-fence                  Option Fence 
      --opt-em-delimiter           Option EmDelimiter 
      --opt-strong-delimiter       Option StrongDelimiter 
      --opt-link-style             Option LinkStyle 
      --opt-link-reference-style   Option LinkReferenceStyle 
      --opt-escape-mode            Option EscapeMode 

  -A, --plugin-conf-attachment     Plugin ConfluenceAttachments 
  -C, --plugin-conf-code           Plugin ConfluenceCodeBlock 
  -F, --plugin-frontmatter         Plugin FrontMatter 
  -G, --plugin-gfm                 Plugin GitHubFlavored 
  -S, --plugin-strikethrough       Plugin Strikethrough 
  -T, --plugin-table               Plugin Table 
      --plugin-table-compat        Plugin TableCompat 
  -L, --plugin-task-list           Plugin TaskListItems 
  -V, --plugin-vimeo               Plugin VimeoEmbed 
  -Y, --plugin-youtube             Plugin YoutubeEmbed
```

### Examples

#### Simplest form

```md
$ html2md -i https://github.com/suntong/html2md | head -3
[Skip to content](#start-of-content)

[Homepage](https://github.com/)
```

#### Using goquery

The most useful feature is to use and pass a [goquery](https://github.com/PuerkitoBio/goquery) selection to filter for the content you want. 

```md
$ html2md -i https://github.com/JohannesKaufmann/html-to-markdown -s "div.BorderGrid-row.hide-sm.hide-md > div"
```


### The options and plugins

Works as expected:

```sh
$ echo '<strong>Bold Text</strong>' | html2md -i
**Bold Text**

$ echo '<strong>Bold Text</strong>' | html2md -i --opt-strong-delimiter="__"
__Bold Text__


$ echo '<ul><li><input type=checkbox checked>Checked!</li><li><input type=checkbox>Check Me!</li></ul>' | html2md -i -G
- [x] Checked!
- [ ] Check Me!

$ echo 'Only <del>blue ones</del> <s> left</s>' | html2md -i --plugin-strikethrough
Only ~blue ones~ ~left~
```

#### Testing the new table plugins

```sh
$ cat $GOPATH/src/github.com/JohannesKaufmann/html-to-markdown/testdata/TestPlugins/table/input.html | html2md -i -T | head -6
| Firstname | Lastname | Age |
| --- | --- | --- |
| Jill | Smith | 50 |
| Eve | Jackson | 94 |
| Empty |  |  |
| End |

$ cat $GOPATH/src/github.com/JohannesKaufmann/html-to-markdown/testdata/TestPlugins/table/input.html | html2md -i -T --domain example.com | diff -wU 1 $GOPATH/src/github.com/JohannesKaufmann/html-to-markdown/testdata/TestPlugins/table/output.table.golden -
---
@@ -41 +41,2 @@
 | `var` | b | c |
\ No newline at end of file
+

$ cat $GOPATH/src/github.com/JohannesKaufmann/html-to-markdown/testdata/TestPlugins/table/input.html | html2md -i --plugin-table-compat | head -6
Firstname · Lastname · Age

Jill · Smith · 50

Eve · Jackson · 94

$ cat $GOPATH/src/github.com/JohannesKaufmann/html-to-markdown/testdata/TestPlugins/table/input.html | html2md -i --plugin-table-compat --domain example.com | diff -wU 1 $GOPATH/src/github.com/JohannesKaufmann/html-to-markdown/testdata/TestPlugins/table/output.tablecompat.golden -
---
@@ -41 +41,2 @@
 `var` · b · c
\ No newline at end of file
+
```

## Credits


### Credits

- [Johannes Kaufmann's html-to-markdown](https://github.com/JohannesKaufmann/html-to-markdown) that does the heavy lifting behind the scene.

### Similar Projects

- [turndown (js)](https://github.com/domchristie/turndown), a very good library written in javascript.
- [lunny/html2md](https://github.com/lunny/html2md), which is using [regex instead of goquery](https://stackoverflow.com/a/1732454), which exhibits a few edge cases which prompted `github.com/JohannesKaufmann/html-to-markdown`
- [jaytaylor/html2text](https://github.com/jaytaylor/html2text), which is not converting to markdown but plain text.

## Download/install binaries

- The latest binary executables are available 
as the result of the Continuous-Integration (CI) process.
- I.e., they are built automatically right from the source code at every git release by [GitHub Actions](https://docs.github.com/en/actions).
- There are two ways to get/install such binary executables
  * Using the **binary executables** directly, or
  * Using **packages** for your distro

### The binary executables

- The latest binary executables are directly available under  
https://github.com/suntong/html2md/releases/latest 
- Pick & choose the one that suits your OS and its architecture. E.g., for Linux, it would be the `html2md_verxx_linux_amd64.tar.gz` file. 
- Available OS for binary executables are
  * Linux
  * Mac OS (darwin)
  * Windows
- If your OS and its architecture is not available in the download list, please let me know and I'll add it.
- The manual installation is just to unpack it and move/copy the binary executable to somewhere in `PATH`. For example,

``` sh
tar -xvf html2md_*_linux_amd64.tar.gz
sudo mv -v html2md_*_linux_amd64/html2md /usr/local/bin/
rmdir -v html2md_*_linux_amd64
```


### Distro package

- [Packages available for Linux distros](https://cloudsmith.io/~suntong/repos/repo/packages/) are
  * [Alpine Linux](https://cloudsmith.io/~suntong/repos/repo/setup/#formats-alpine)
  * [Debian](https://cloudsmith.io/~suntong/repos/repo/setup/#formats-deb)
  * [RedHat](https://cloudsmith.io/~suntong/repos/repo/setup/#formats-rpm)

The repo setup instruction url has been given above.
For example, for [Debian](https://cloudsmith.io/~suntong/repos/repo/setup/#formats-deb) --

### Debian package


```sh
curl -1sLf \
  'https://dl.cloudsmith.io/public/suntong/repo/setup.deb.sh' \
  | sudo -E bash

# That's it. You then can do your normal operations, like

sudo apt-get update
apt-cache policy html2md

sudo apt-get install -y html2md
```

## Install Source

To install the source code instead:

```
go install github.com/suntong/html2md
```

## Author

Tong SUN  
![suntong from cpan.org](https://img.shields.io/badge/suntong-%40cpan.org-lightgrey.svg "suntong from cpan.org")

_Powered by_ [**WireFrame**](https://github.com/go-easygen/wireframe)  
[![PoweredBy WireFrame](https://github.com/go-easygen/wireframe/blob/master/PoweredBy-WireFrame-Y.svg)](http://godoc.org/github.com/go-easygen/wireframe)  
the _one-stop wire-framing solution_ for Go cli based projects, from _init_ to _deploy_.

## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="https://github.com/suntong"><img src="https://avatars.githubusercontent.com/u/422244?v=4?s=100" width="100px;" alt=""/><br /><sub><b>suntong</b></sub></a><br /><a href="https://github.com/go-cc/cc2py2/commits?author=suntong" title="Code">💻</a> <a href="#ideas-suntong" title="Ideas, Planning, & Feedback">🤔</a> <a href="#design-suntong" title="Design">🎨</a> <a href="#data-suntong" title="Data">🔣</a> <a href="https://github.com/go-cc/cc2py2/commits?author=suntong" title="Tests">⚠️</a> <a href="https://github.com/go-cc/cc2py2/issues?q=author%3Asuntong" title="Bug reports">🐛</a> <a href="https://github.com/go-cc/cc2py2/commits?author=suntong" title="Documentation">📖</a> <a href="#blog-suntong" title="Blogposts">📝</a> <a href="#example-suntong" title="Examples">💡</a> <a href="#tutorial-suntong" title="Tutorials">✅</a> <a href="#tool-suntong" title="Tools">🔧</a> <a href="#platform-suntong" title="Packaging/porting to new platform">📦</a> <a href="https://github.com/go-cc/cc2py2/pulls?q=is%3Apr+reviewed-by%3Asuntong" title="Reviewed Pull Requests">👀</a> <a href="#question-suntong" title="Answering Questions">💬</a> <a href="#maintenance-suntong" title="Maintenance">🚧</a> <a href="#infra-suntong" title="Infrastructure (Hosting, Build-Tools, etc)">🚇</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
