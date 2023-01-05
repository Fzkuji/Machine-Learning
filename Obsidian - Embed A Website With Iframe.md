Embed files

### Embed attachments

You can embed attachment files like images or audio in your notes. Use the `![[filename.png]]` syntax like so:

![Engelbart.jpg](https://publish-01.obsidian.md/access/f786db9fac45774fa4f0d8112e232d67/Attachments/Engelbart.jpg)

You can embed a PDF file in your notes with the same syntax. Additionally, you can write `![[My File.pdf#page=number]]` to open to that specific page of the PDF directly.

### Embed notes

You can embed a note also, with the same syntax:

Accepted file formats

Obsidian recognizes the following file formats right now:

1.  Markdown files: `md`;
2.  Image files: `png`, `jpg`, `jpeg`, `gif`, `bmp`, `svg`;
3.  Audio files: `mp3`, `webm`, `wav`, `m4a`, `ogg`, `3gp`, `flac`;
4.  Video files: `mp4`, `webm`, `ogv`, `mov`, `mkv`;
5.  PDF files: `pdf`.

All these types of files can be [embedded](https://help.obsidian.md/How+to/Embed+files) in a note.

Note that audio and video formats support depend on the availability of codecs on your device.

### iframe

"iframe" is a way to embed a web page in another. It's useful because Markdown can accept HTML, which is a simple language to construct the web pages we see every day.

For example:

```html
<iframe src="https://www.youtube.com/embed/NnTvZWp5Q7o"></iframe>
```

Produces:

The basic syntax is:

```html
<iframe src="INSERT YOUR URL HERE"></iframe>
```

Some websites have quirks that don't allow you to embed them. For example, you can't embed a YouTube video by using its normal URL, but you can use its embed URL which is `https://www.youtube.com/embed/VIDEO_ID`.

If you want to embed a website, try searching for "{website} embed iframe".

For example, you can embed Twitter tweets like so as suggested by search results:

```html
<iframe
	border=0
	frameborder=0
	height=250
	width=550  
	src="https://twitframe.com/show?url=https%3A%2F%2Ftwitter.com%2Fjack%2Fstatus%2F20">
</iframe>
```

---

### Developer notes

The syntax is inspired by Markdown's image syntax. We had two observations:

1.  `[Image](link.png)` links to the image, but `![Image](link.png)` actually displays it.
    
2.  The internal link syntax is based on this assumption: `[[My page]]` is a shortcut (or "syntactic sugar" if you will) for `[My page](My%20page.md)`.
    

To combine these two things, we can come up with a third pattern:`![[My page]]` should be equivalent to `![My page](My%20page.md)` , which by the image convention, if the title is the same as the link, should display it.

There are other implementations out there, for example Roam Research uses `{{embed: ((NODE_ID))}}`. We have also considered using the `((Page name))` syntax too, but we feel like `![[Page name]]` is more consistent, more familiar, and leaves more room so that the other symbols to have their own meanings.

#### Resize images

You can resize images using the following syntax:

For markdown images, use `![AltText|100x100](https://url/to/image.png)`

For embeds, use `![[image.png|100x100]]`

To have the image scale according to its aspect ratio, omit the height `![[image.png|100]]`

LINKS TO THIS PAGE

[Accepted file formats](https://help.obsidian.md/Advanced+topics/Accepted+file+formats)

[Audio recorder](https://help.obsidian.md/Plugins/Audio+recorder)

[Format your notes](https://help.obsidian.md/How+to/Format+your+notes)

[Index](https://help.obsidian.md/Obsidian/Index)

[Note composer](https://help.obsidian.md/Plugins/Note+composer)

[Start here](https://help.obsidian.md/Start+here)

[Use callouts](https://help.obsidian.md/How+to/Use+callouts)