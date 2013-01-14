# Gekyll

Gekyll is a [Jekyll](https://github.com/mojombo/jekyll/) plugin that lets you use Git repositories as posts, giving you access to a post's commits, diffs, and more.

## Getting Started

- Copy `gekyll.rb` from this repository into your Jekyll project's `_plugins` directory.
- Gekyll depends on the [Grit](https://github.com/mojombo/grit/) gem. So install it: `gem install grit`.

## Adding Git repositories as Jekyll posts

- Gekyll intercepts all "XXXXX.git" directories in your Jekyll project's `_posts/` directory. Gekyll needs those to be "bare" repositories. So when you want to use a repo as a post, you'll want to `git clone --bare {REPO LOCATION} _posts/{DESIRED SLUG}.git`. 
- To get the main post content, Gekyll looks in your repository's ground-floor directory for — in order — the first file named draft.EXTENSION, readme.EXTENSION, FILENAME.md, FILENAME.mkd, FILENAME.markdown, FILENAME.txt. (You can change this order in your `_config.yml` file; see the Configuration section below for details.)
- Gekyll then treats this file much like it would any standard post in Jekyl, extracting a YAML header, adding it to the full list of posts, et cetera.
- Gekyll considers the date of your most recent commit to be the post's "date," unless you specify a date in the post's YAML header.

## Extra Files

In addition to rendering the post, Gekyll also spits out the following files:

	- `SLUG.git`: a raw, bare fork of your repo, clonable even from a static HTTP server.
	- `SLUG/raw/*`: a directory containing all the raw files in your repo
	- `SLUG/commits.json`: a JSON file containing information about all commits to this repo.
	- `SLUG/diffs/*`: a directory containing one JSON file per commit, each representing an array of all diffs in that commit.

These files let you do fun things with your post/repo. But creating them can slow Gekyll down. So if you don't want them, you can tell Gekyll in `_config.yml` not to write them. See the Configuration section below for details.

## Layouts & Templating

In addition to whatever you place in your YAML data, you'll have access to:

- `is_repo`: a boolean, always true.
- `commits`: an array of hashes representing each repo commit, in reverse-chronological order.
- `date`: defaults to the date of the most recent commit, but can be overridden/hardcoded in a post's YAML header.
- `first_commit_date`: the date of the repo's first commit.
- `last_commit_date`: the date of the repo's most recent commit. (Same as `date` unless `date` has been overriden.)

See `example_layouts/crude-repo-page.html`

## Configuration

Gekyll looks for a "gekyll" section in your Jekyll project's `_config.yml` file. Currently, it supports the following key/values:

- `filename_matches`: a list/array of case-insensitive, extensionless filenames Gekyll should consider to contain your main post, in order of priority. Default: `[ draft, readme ]`
- `extension_matches`: a list/array of case-insensitive, filenameless extensions Gekyll should consider to contain your main post, in order of priority. Default: `[ md, mkd, markdown, txt ]`
- `extras`: a list/array of the extra files (see the Extra Files section above) that Gekyll should generate. Default: `[ repo, blobs, commits, diffs ]`
- `default_layout`: the Jekyll layout that Gekyll should apply to its posts by default — can be overridden in the post's YAML header. Default: `repo`

So, if you wanted to use "article.EXTENSION" the main file to be rendered, and you didn't want Gekyll to write the raw repo or raw  repo files, you'd add these lines to your `_config.yml`:

```
	filename_matches:
		- article
	extras:
		- commits
 		- diffs
```

## TODO

- Add tests.
- Consider making more aspects of each repository available to Liquid templates.
- [Your idea here.]