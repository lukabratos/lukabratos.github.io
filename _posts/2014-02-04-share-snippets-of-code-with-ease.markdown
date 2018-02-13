---
layout: post
title: "Share code snippets with ease"
date: 2014-02-04 11:19:06 +0000
---
**Update - Snippet was rewritten in Python! See [this](https://github.com/lukabratos/Snippet/issues/2)**.

[GitHub Gist](https://help.github.com/articles/creating-gists) is a nice way to share a snippet of code or a block of text with other people. Gists are under Git revision control so it is very easy to view and follow changes, other users can fork them and they are under SSL encryption for private sharing.

## Overview

Snippet is a Mac OS X Service made with Automator and powered by a small Ruby script for uploading code snippets to GitHub Gist with the ease of a simple right-click.

## Implementation

Implementation was pretty straightforward. Gists can be created for anonymous users without a token.
Code bellow will create private Gist named `file1.txt` with content `Hello World!`.
We can optionally add a description and set visibility of Gist to public or private. The default is private.

```ruby
curl -X POST \
--data-binary '{"files": {"text.txt": {"content": "Hello World!"}}}' \
https://api.github.com/gists
```

I used Automator workflow with Shell Script and Ruby to achieve my goal with the help of a simple Ruby script that will take selected text, create an API call and parse the URL value returned by the GitHub API and copy it to the clipboard.

```ruby
require 'json'
require 'net/http'

uri = URI("https://api.github.com/gists")
data = { 'files' => { 'snippet' => { 'content' => ARGV[0] }}}

request = Net::HTTP::Post.new(uri.path)
request.body = data.to_json

response = Net::HTTP.start(uri.hostname, uri.port, :use_ssl => true) do |http|
  http.request(request)
end

my_hash = JSON.parse(response.body)
IO.popen('pbcopy', 'w') { |f| f << my_hash["html_url"] }
```

## Installation

Double-click on `Snippet.workflow` and click on the `Install` button.
This will install Snippet into `~/Library/Services` folder.

You can uninstall by deleting it from the same folder.

If you want to assign a keyboard shortcut to Snippet service you can do this easily by assigning a key in `System Preferences -> Keyboard -> Shortcuts -> Services`.

## Usage

Select code and right-click. `Snippet` is located under `Services` menu.
When you use it, it will automatically upload selected text to GitHub Gist and it will copy generated URL to your clipboard so you can share instantly.

## Requirenments

`Ruby 1.9.2+` or `gem install json`.

You can download the project [here](https://github.com/lukabratos/Snippet).
