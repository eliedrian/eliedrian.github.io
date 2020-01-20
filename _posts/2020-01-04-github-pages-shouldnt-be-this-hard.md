---
layout: post
title:	"Blogging With GitHub Pages Shouldn't Be This Hard"
date: 2020-01-04 20:20 +0800
categories: howto
---

[GitHub Pages][github-pages] is a free static web hosting service offered by GitHub to its users.
It's actually quite simple to use.
You basically push files to a special repository or branch and GitHub will automatically host those for you.

GitHub Pages also graciously offers support for [Jekyll][jekyll].

Jekyll is a popular static website generator, typically used for personal, project, or organization sites.
Put simply, it's a file-based content management system.
This makes it quite perfect for blogs, such as this one!

Here's where my problem lies---setting it up is a tad too tedious.
GitHub does its best to provide us with documentation, but some commands with Jekyll and Bundler just don't work!
So after a couple of minutes (or even hours) of head-scratching trying to do it the right way, I think I've found *a* way to do it right.
So in true [new standard][xkcd-standard] fashion, here's how *I* setup Jekyll with GitHub Pages.

![xkcd comic on standards][xkcd-standard-img]

### Creating your Jekyll site

So you've created your *fresh* repository; now it's time to create the actual site.
For that, we'll need [Ruby][ruby] and [Bundler][bundler] installed---these are required for Jekyll to run.

1. Create a new directory for our site to live in.

	~~~ shell
	$ mkdir <user>.github.io
	~~~

	`cd` into our new directory and let's begin.

2. Configure Bundler.
	
	Configure Bundler to install gems inside our fresh directory.

	~~~ shell
	$ bundle config set path vendor
	~~~

	Create a blank Gemfile for us to add our site dependencies.

	~~~ shell
	$ bundle init
	~~~

3. Install the `github-pages` gem.
	
	The `github-pages` gem is a collection of gems that GitHub *needs* to build Jekyll sites.

	~~~ shell
	$ bundle add github-pages
	~~~

4. Create new Jekyll site.

	~~~ shell
	$ bundle exec jekyll new . --force
	~~~

	`--force` is needed since the directory is not empty.

5. Update Gemfile.

	The Gemfile will need to be updated, since it's been rewritten by the Jekyll new site command.
	Comment the line containing "jekyll" and uncomment the line with "github-pages".
	
	**NOTE:** As of January 3, 2020, another little tweak has to be done to make Jekyll play nice.
	This line has to be added to the Gemfile:

	~~~ ruby
	gem "faraday", "~> 0"
	~~~

	Hopefully a fix comes in the next version of the `github-pages` gem.

6. Update bundle.

	~~~ shell
	$ bundle update
	~~~

7. Test out the Jekyll site.

	~~~ shell
	$ bundle exec jekyll serve
	~~~

	This will serve our brand-new Jekyll site at <http://localhost:4000>, so give that a visit and look in awe at your new site!

Next, we have to create the repository for our new site to live in.

### Creating a special repository

Instead of reinventing the wheel, I'll link to the official documentation on how to create the repository instead---found [here][create-repository-doc]. 😉

After creating our special repository, we can now push our site to GitHub with these steps.

1. Initialize a git repository

	~~~ shell
	$ git init
	~~~

2. Set git remote

	~~~ shell
	$ git remote add origin <git repository>
	# For example: git remote add origin git@github.com:eliedrian/eliedrian.github.io.git
	~~~

3. Commit files to git

	~~~ shell
	$ git add . && git commit -m "Initial commit"
	~~~

4. Push files to GitHub

	~~~ shell
	$ git push -u origin master
	~~~

5. ???

6. Profit!

	Your site should finally be on it's way to being published on GitHub!
	It'll be found at https://\<user\>.github.io!
	If this is your first time using GitHub pages, it might take a couple of minutes before it actually becomes live on the Internet.

### What's next?

Start writing your *own* content with Jekyll, by following [this][adding-content-doc].
Change the theme by following [this][theme-customization-doc], or reading `_config.yml` for some site-wide variables you can set.
If you get lost you can *always* refer to the [Jekyll docs][jekyll-docs].

Happy posting!

[adding-content-doc]: https://help.github.com/en/github/working-with-github-pages/adding-content-to-your-github-pages-site-using-jekyll#adding-a-new-post-to-your-site
[bundler]: https://bundler.io/
[create-repository-doc]: https://help.github.com/en/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll#creating-a-repository-for-your-site
[github-pages]: https://pages.github.com/
[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll]: https://jekyllrb.com/
[ruby]: https://www.ruby-lang.org/en/
[theme-customization-doc]: https://help.github.com/en/github/working-with-github-pages/adding-a-theme-to-your-github-pages-site-using-jekyll
[xkcd-standard-img]: https://imgs.xkcd.com/comics/standards.png 
[xkcd-standard]: https://xkcd.com/927/
