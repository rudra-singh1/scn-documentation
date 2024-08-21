---
title: Contribute to SCN Docs
---

# Contribute to SCN Docs

## Looking to help?

If you wanna share resources and help improve our docs, this page will get you started!
Our docs are designed so that anyone can contribute. If this page isn't enough, contact one of
us and we'll be able to help you!

Our documentation uses MkDocs ReadTheDocs theme (links at the bottom of the page)

## Editing process
To edit this documentation you should:

1. Get your own copy of the [repo](https://github.com/Local-Connectivity-Lab/scn-documentation)
1. Modify the documentation in your own repo (for more information see section on Local Development)
1. Submit a pull request
1. Wait for someone to review and approve the request. You can also nudge someone who has access to approve it.

The rest of this page will explain all the details you need to know about the directory structure, markdown, and other quirks for editing this documentation. Make sure to read everything!

### Markdown files
All our documentation is stored in 'Markdown' files so that they can be easily
modified and changed without heavy technical knowledge.

#### Markdown Editors

- A nice and simple online editor is [StackEdit](https://stackedit.io/app#) which will let you type in markdown and see what it would look like in realtime in split-screen.

- Another option is [HackMD](https://hackmd.io/) which has the same features as StackEdit
but it also allows you to connect to your own GitHub repo and pull/push.
	- This is a nice option if you are uncomfortable with using Git from the command line.

### Using MkDocs
[MkDocs](https://www.mkdocs.org/) is pretty simple, just install it through pip then you can run `mkdocs serve` to locally view the website it will generate.

Each directory has a `.pages` file that declares the order each file/folder in that directory will show up.
Children pages are implicit in the directory structure

If you need static files for any of your pages, you should put them in the `assets` folder within the top level `docs` folder

For example, for the cable-crimping page, the images for the tutorial are located in the "assets/cable-crimping" folder. These images can then be referenced relatively with the following standard markdown image syntax:
```markdown
![RJ45 Crimping Tool](../assets/cable-crimping/kit-crimping-tool.jpg)
```

## Deployment
The default branch of the repo is `gh-pages-src` which is where development should be done. 
One of the things that means is that pull requests will request to be merged by default into this branch. 

When a merge happens, a github workflow is set up (see .github/workflows) to listen to new commits, and then transform the source to something that can be served by a static file server.
These build artifacts are automatically comitted to the branch `gh-pages` whose latest commit is what our documentation site serves.
