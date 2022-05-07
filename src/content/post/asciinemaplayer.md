---
title: "Asciinemaplayer"
date: 2021-06-11
tags : [ "asciinema"]
categories : ["hugo"]
author: "sparrowyang"
draft: false 
---

[asciinema home page](https://asciinema.org/)
## Record a cast
install a asciinema in you computer, and record a cast

```
asciinema rec hello.cast
```

copy `hello.cast` to `path-to-your-hugo/static/ascrec/`

## Use shortcode in your Markdown file
``` 
[[% asciinema cf="hello.cast" speed="2" theme="solarized-light" rows="20"  %]]
```
(To avoid Render, I use '[' to replace '{', you must replce again! )


result:

{{% asciinema cf="hello.cast" speed="2" theme="solarized-light" rows="20"   %}}
