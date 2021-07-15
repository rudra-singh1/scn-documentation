---
title: Contribute to SCN Docs
---

# Contribute to SCN Docs

## Looking to help?

If you wanna share resources and help improve our docs, this page will get you started!
Our docs are designed so that anyone can contribute. If this page isn't enough, contact one of
us and we'll be able to help you!

Our documentation uses the [Just the Docs](https://pmarsceill.github.io/just-the-docs/) Jekyll theme.
This theme provides a lot of nice features, so look there for help too!

## Editing process
To edit this documentation you should:
1. Get your own copy of the [repo](https://github.com/Local-Connectivity-Lab/scn-documentation)
2. Modify the documentation in your own repo (for more information see section on Local Development)
3. Submit a pull request
4. Wait for someone to review and accept the request

The rest of this page will explain all the details you need to know about the directory structure, markdown, and other quirks for editing this documentation. Make sure to read everything!

**PRO TIP**{: .label .label-green } If you want to quickly edit a page, you can scroll to the bottom of the page and click "Edit this page on GitHub". Clicking the pencil button on that page  will automatically send you to your own fork, allow you to make edits, and you can make a pull request from there.

## Markdown files
All our documentation is stored in 'Markdown' files so that they can be easily
modified and changed without heavy technical knowledge.

### Markdown Editors

- A nice and simple online editor is [StackEdit](https://stackedit.io/app#) which will let you type in markdown and see what it would look like in realtime in split-screen.

- Another option is [HackMD](https://hackmd.io/) which has the same features as StackEdit
but it also allows you to connect to your own GitHub repo and pull/push.
	- This is a nice option if you are uncomfortable with using Git from the command line.

## Local Development

### Using Jekyll
The best workflow for editing our documentation is to get Jekyll installed on your computer so that you can dynamically generate this website and see your changes on your own computer. The only downside is that it requires some set-up, but once everything is setup everything should be smooth!

You must have Jekyll installed (instructions for your respective OS can be found [here](https://jekyllrb.com/docs/installation/)).

After you have Jekyll installed, you can start up a local web server by running the command `bundle exec jekyll serve` in the main directory of this repo on your computer. This should spin up a webserver at http://localhost:4000/ and you can make edits to files and refresh your page to see the changes.

Once you're satisfied with your changes, make sure all your changes are committed to your repo and submit a pull request!

### Avoiding Jekyll
If you're planning on just making small edits to markdown files, then you don't _**have**_ to install Jekyll to make changes. You can simply edit markdown files, push it to your repo, and preview your changes in the github page for your respective repo (GitHub should automatically deploy it for you with a personalized link) If you're satisfied with these changes, go ahead with submitting a pull request!

### A Note on GitHub Deployments
The best way to preview what the website will look like is to use Jekyll and see what it looks like on the localhost webserver.

However, if you push changes to your local repo, GitHub will automatically deploy a version of the page and give you apersonalized link. This deployment will not be accurate because you need to change the "url" in \_config.yml to reflecet the personalized link for your github repo. If you don't, all the links and assets will point to stuff from docs.seattlecommunitynetwork.org.

## Documentation Directory Structure

### Top-level pages
An example of a 'top-level page' would be the 'Get Started' page.
The 'Get Started' page is located in the top-level directory inside the markdown file
'get-started.md'. Note that the file name doesn't affect anything but the page's name
in the URL (get-started.html). Everything else is controlled by the stuff inside
the 'YAML Front Matter' of the markdown file.

Here is the YAML front matter for 'Get Started':
```yaml
---
title: Get Started
nav_order: 2
---
```

- The title represents the display name of the page and is important for connecting it with other pages.
- The nav_order dictates that this page is the 2nd from the top in the sidebar.

### Parent pages
An example of a 'parent page' would be the 'Learn' page.
The 'Learn' page is located in the learn directory as 'learn/index.md'.
Note that the 'Learn' page is almost the same as a top-level page, except it has children pages.

Here is the YAML front matter for 'Learn'
```yaml
---
title: Learn
nav_order: 4
has_children: true
---
```

- The 'title' represents the display name of this page and is also what the children
pages will use to reference this page as their parent.
- The 'has_children' is what dictates 'Learn' to be a parent page.

### Children pages
An example of a 'children page' would be the 'Wireless Communication' page inside the learn directory as 'learn/wireless-communication.md'.

Here is the YAML front matter for 'Wireless Communication':
```yaml
---
title: Wireless Communication
parent: Learn
nav_order: 1
---
```

- The 'parent' is used to mark the 'Learn' page as a parent, which causes this page to
appear underneath 'Learn' as a dropdown page.
- The 'nav_order' is used to enforce that this page occurs as the first page underneath 'Learn'

## Static Files

If you need static files for any of your pages, you should put them in the "assets" folder underneath an appropriate folder.

For example, for the cable-crimping page, the images for the tutorial are located in the "assets/cable-crimping" folder. These images can then be referenced with the following standard markdown image syntax:
```markdown
{% raw %} ![RJ45 Crimping Tool]({{site.url}}/assets/cable-crimping/kit-crimping-tool.jpg) {% endraw %}
```
**NOTE**{: .label .label-green} Notice the use of site.url with the curly braces. This is 'Liquid' syntax that is used by Jekyll to dynamically generate the base url. This should be docs.seattlecommunitynetwork.org in production, but will be localhost:4000 when ran locally with `bundle exec jekyll serve`.
