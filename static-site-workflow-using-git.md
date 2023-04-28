+++
title = "Using Git in Your Static Site Workflow"
description = "How you can use git in your static site development, testing, and production workflow."
template = "blog_post.html"
date = 2023-04-11
draft = true
[taxonomies]
tags = ["git", "webdev", "zola", "devops"]
+++
This website and its contents are deployed to my VPS via git. The reposidtory is pushed to a production remote and uses hooks to run some basic scripts that organize the incoming content. This is _almost_ CI/CD, but not quite; regardless, git can be a powerfull tool to build simple deployment pipelines to remote servers.
<!-- more -->

