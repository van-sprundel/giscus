+++
title = "A data-oriented resume"
description = "Tired of browsing Word themes?"
date = "2024-09-16"

[taxonomies]
tags = ["json", "jsonresume", "resume"]
+++

I'm sure you've all been there—spending 20 minutes digging through folders for that outdated Word document containing your already-deprecated resume. After an inevitable battle with Word’s layout system, which somehow manages to derail whatever initial concept you had in mind, you begin to wonder if there's another tool designed for this. 

I don't blame Word though, it's just the only application I know that's decent at making documents. I've tried a few online editors, but they have the same problem as any other "free" product. Either it's paywalled, watermarked or extremely limited. And don't get me started on making an account for some client-side application...

## Ever thought of re-inventing the wheel?
Hear me out. What if we approached it as a software dev? Let's split the resume into two parts. We have a stored object (JSON, TOML, YAML...) that contains all the information we'd want to display, and we can pass it through some other tool to generate a nice-looking resume.

Well, as with any cool idea, someone was ahead of you. The [JSON resume](https://jsonresume.org/)project focuses on an open-source initiative to create a JSON-based standard for resumes. Lucky for us, because we can just insert our own data into the [predefined schema](https://jsonresume.org/schema). There's even a [LinkedIn importer](https://github.com/joshuatz/linkedin-to-jsonresume)!

Setting up my own resume only took around an hour. You can store your resume on [Gist](https://gist.github.com), and tweak your resume whenever you please.

### How's this any different to an online editor?
If you like to hack together your own tools, you probably realized the power of a resume standard. Now you’ve got your entire resume packaged inside a structured object, which can be consumed by any tool that understands the format.

### Alright, but what can I do with it?
You can now serve your resume as a single HTML file and some CSS, or you could build an entire portfolio on top of your resume. How about an embedded project, where you display an entire resume on a screen, stored entirely in-memory?

{{ note(header="Note:", body="I'm not an embedded engineer, you guys just make lights blink all day right? (jk)") }}

### But wait... what about PDFs?
Ah yeah, links to resumes are more often than not disregarded by recruiters. Luckily [puppeteer](https://pptr.dev/) exists. Exporting your resume to a PDF only takes a few lines of JS:

```js
import puppeteer from 'puppeteer'

const browser = await puppeteer.launch()
const page = await browser.newPage()

const url = 'https://registry.jsonresume.org/van-sprundel'; // make sure you have a gist!
await page.goto(url, { waitUntil: 'networkidle0' });
await page.pdf({ path: 'resume.pdf', format: 'a4', printBackground: true })
await browser.close()
```
The power of bodgeing!

## Shameless self-insert
I haven't found any CLI tool that fills all my needs yet, so I ended up implementing my own, called [ferrisume](https://github.com/van-sprundel/ferrisume)(ferris + resume). If you have a bug (or maybe some critique) make sure you do it in an issue. 