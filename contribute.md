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
2. Modify the documentation in your own repo
3. Submit a pull request
4. Wait for someone to review and accept the request

**PRO TIP**{: .label .label-green } If you want to quickly edit a page, you can scroll to the bottom of the page and click "Edit this page on GitHub". Clicking the pencil button on that page  will automatically send you to your own fork, allow you to make edits, and you can make a pull request from there.

## Markdown files
All our documentation is stored in 'Markdown' files so that they can be easily
modified and changed without heavy technical knowledge.

### Markdown Editors

- A nice and simple online editor is [StackEdit](https://stackedit.io/app#) which will let you type in markdown and see what it would look like in realtime in split-screen.

- Another option is [HackMD](https://hackmd.io/) which has the same features as StackEdit
but it also allows you to connect to your own GitHub repo and pull/push.
	- This is a nice option if you are uncomfortable with using Git from the command line.

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

TODO
{: .label .label-yellow }
