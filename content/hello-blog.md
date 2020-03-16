+++
title = "Hello, world!"
date = 2020-03-13
+++
Welcome to my blog!

I'm still warming up my keyboard, but it feels right to start the blog
with writing a few words on how this whole thing works.

<!-- more -->

<hr>

It's never been easier to have you own website, as long as it's purely for the purpose
of sharing your content with readers. When you don't need to worry about authentication,
security and dynamically generated content, things get vastly simplified. And basically free.

If you want to read more about static vs dynamic sites, here's
[a nice write-up from GitLab](https://about.gitlab.com/blog/2016/06/03/ssg-overview-gitlab-pages-part-1-dynamic-x-static/).

### The content

In order to publish a website, you obviously need to have some content.
The rest depends on how much you'd like to be involved in the low-level stuff and what's your background.

<center>
<div>
    <img src="https://imgs.xkcd.com/comics/blogging.png" alt="xkcd on blogging"/>
    <a href="https://xkcd.com/741/"><em>mandatory xkcd comic</em></a>
</div>
</center>
<br>

You could, for example, hand-craft each HTML tag. It's a nice exercise when first learning HTML,
but it's an extremely tedious process. I suppose this method is mostly for school assignments.

On the other end of the spectrum, there's a plethora of modern web frameworks. If you're a web developer,
that's a natural choice - but you don't need me to tell you that. If you're not - learning a whole web framework
is a pretty big time investment and - honestly - seems to be an overkill for a site like this one.

You could also use one of commercial websites builders, which come with all the batteries included.
These batteries don't come free, though. It's the easiest option, but that's not always a virtue
\- for example I didn't want to drag-and-drop buttons, I wanted to have a saying in my templates and layout.

Enter the static site generators. The idea is pretty simple - pick a generator, 
write out the content of your page in a Markdown, mix in some configuration files 
according to the documentation and let the tool of your choice do the rest.
I chose [Zola](https://www.getzola.org/) - for no particular reason except it being written in Rust.

This way I could write all the content in Markdown, but drop down to HTML and CSS when I want to adjust something.

But once Zola builds your website, you need to host it somewhere.

### Hosting

There are many hosting options out there, either free or very cheap:
- [GitLab Pages](https://docs.gitlab.com/ee/user/project/pages/),
- [Netlify](https://templates.netlify.com/),
- [AWS S3](https://aws.amazon.com/getting-started/projects/host-static-website/),
- and finally [Github Pages](https://pages.github.com/) - the one I'm using right now.

I find it amazing how easy it is to have a demo website published.
It's literally a few clicks away - and you get a published website with which you can experiment any way you way.
It may not be very original, but hey - at least it's pretty!

Since I'm using Zola instead of the [current king of SSGs](https://jekyllrb.com/),
I decided to start from scratch and follow the instructions from
[Zola documentation](https://www.getzola.org/documentation/deployment/github-pages/).
These docs also describe deployment using GitLab and Netlify.

### Continuous integration

GitHub Pages deployment is all nice, but it requires the website to be already built on master.
I could do `zola build`, commit and push the files myself, but why waste keystrokes
when Jenkins' colleague - Travis -  will do that for me?

Especially considering that Travis CI is free for open source projects.

So my workflow looks like this:
- I scribble a new post in Markdown
- I push to `code` branch (or to another branch if it's a draft)
- Travis hook is called upon each push
- it builds the website (environment setup + `zola build`)
- upon success, it pushes the results to `master`
- GitHub serves the website using files from `master`

Having my CI experience limited to Jenkins, I was positively surprised by the simplicity
and conciseness of `.travis.yml`:

<script src="https://gist.github.com/smwoj/bd9cebf67e195c5163c3ff637258c4b2.js"></script>

Most of this yaml is taken from
[zola deployment instructions](https://www.getzola.org/documentation/deployment/github-pages/).
The only catch here is that - as I'm writing this - Travis by default uses VM with Ubuntu 16.04
to execute the builds. It turns out this image has some issues with libssl shared library,
which Zola depends on. After a few failed attempts to fix this problem in the default image
I tried using Ubuntu 18.04 instead. It worked just fine (hence `dist: bionic` in the yaml).

That's all for the short overview of the technical side of this blog. 
If you'd like to check the sources - [here they are](https://github.com/smwoj/smwoj.github.io/tree/code).

Thanks for dropping by!
