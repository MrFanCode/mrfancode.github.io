
`23-03-2025`




# Blog Setup with hugo in Ubuntu (Linux)
### From Mark Down (.md) file into website


### Required to be installed
- `Go, Git, DartSass`

## First open terminal & Install git and go
`sudo apt update && sudo apt install git golang-go -y`


# Getting started
- Install hugo from the official website. [Download from here](https://github.com/gohugoio/hugo/releases/latest)
- Then check the version if you have download it correctly by typing `hugo version`
- Then do some initial works such as down below:

	### First run this to initialize an site directory
	-  `hugo new site example` (this is to initialize the site directory)
	-  `cd example` (this command will change directory into the exmple directory)
	### Secondly, Initialize an empty Git repository in the current directory.
	- `git init`
	### Then, add an theme into the site directory, example:
	- `git submodule add -f https://github.com/panr/hugo-theme-terminal.git themes/terminal`
	- `for more themes option go to:` 
	[Complete List | Hugo Themes](https://themes.gohugo.io)
	### After that, go to [terminal themes docs](https://github.com/panr/hugo-theme-terminal/tree/master)
	- Just copy the configuration
```toml
baseurl = "/"
languageCode = "en-us"
# Add it only if you keep the theme in the `themes` directory.
# Remove it if you use the theme as a remote Hugo Module.
theme = "terminal"
pagination.pagerSize = 5

# Required for Chroma and the custom syntax highlighting.
[markup.highlight]
  noClasses = false

[params]
  # dir name of your main content (default is `content/posts`).
  # the list of set content will show up on your index page (baseurl).
  contentTypeName = "posts"

  # if you set this to 0, only submenu trigger will be visible
  showMenuItems = 2

  # show selector to switch language
  showLanguageSelector = false

  # set theme to full screen width
  fullWidthTheme = false

  # center theme with default width
  centerTheme = false

  # if your resource directory contains an image called `cover.(jpg|png|webp)`,
  # then the file will be used as a cover automatically.
  # With this option you don't have to put the `cover` param in a front-matter.
  autoCover = true

  # set post to show the last updated
  # If you use git, you can set `enableGitInfo` to `true` and then post will automatically get the last updated
  showLastUpdated = false

  # Provide a string as a prefix for the last update date. By default, it looks like this: 2020-xx-xx [Updated: 2020-xx-xx] :: Author
  # updatedDatePrefix = "Updated"

  # whether to show a page's estimated reading time
  # readingTime = false # default

  # whether to show a table of contents
  # can be overridden in a page's front-matter
  # Toc = false # default

  # set title for the table of contents
  # can be overridden in a page's front-matter
  # TocTitle = "Table of Contents" # default

  # Set date/time format for posts
  # This will impact the date/time displayed on
  # index.html, the posts list page, and on posts themselves
  # This value can also be configured per-post on front matter
  # If you have any issues with the timezone rendering differently
  # than you expected, please ensure your timezone is correctly set
  # on your server.
  # This value can be customized according to Hugo documentation:
  # https://gohugo.io/functions/time/format/
  # Default value (no changes needed):
  # dateFormat = "2006-01-02"
  # Example format, with date, time, and timezone abbreviation:
  # dateFormat = "2006-01-02 3:04:06 PM MST"


[params.twitter]
  # set Twitter handles for Twitter cards
  # see https://developer.twitter.com/en/docs/tweets/optimize-with-cards/guides/getting-started#card-and-content-attribution
  # do not include @
  creator = ""
  site = ""

[languages]
  [languages.en]
    languageName = "English"
    title = "Terminal"

    [languages.en.params]
      subtitle = "A simple, retro theme for Hugo"
      owner = ""
      keywords = ""
      copyright = ""
      menuMore = "Show more"
      readMore = "Read more"
      readOtherPosts = "Read other posts"
      newerPosts = "Newer posts"
      olderPosts = "Older posts"
      missingContentMessage = "Page not found..."
      missingBackButtonLabel = "Back to home page"
      minuteReadingTime = "min read"
      words = "words"

      [languages.en.params.logo]
        logoText = "Terminal"
        logoHomeLink = "/"

      [languages.en.menu]
        [[languages.en.menu.main]]
          identifier = "about"
          name = "About"
          url = "/about"
        [[languages.en.menu.main]]
          identifier = "showcase"
          name = "Showcase"
          url = "/showcase"

[module]
  # In case you would like to make changes to the theme and keep it locally in you repository,
  # uncomment the line below (and correct the local path if necessary).
  # --
  # replacements = "github.com/panr/hugo-theme-terminal/v4 -> themes/terminal"
[[module.imports]]
  path = 'github.com/panr/hugo-theme-terminal/v4'
```

- Remove this from the configuration after copied the configuration into your .toml file

```toml

[module]
  # In case you would like to make changes to the theme and keep it locally in you repository,
  # uncomment the line below (and correct the local path if necessary).
  # --
  # replacements = "github.com/panr/hugo-theme-terminal/v4 -> themes/terminal"
[[module.imports]]
  path = 'github.com/panr/hugo-theme-terminal/v4'


```

---

## Try run the server with this command:
`hugo server`
or run this if you want to run the site outside from your localhost
`hugo server --bind 0.0.0.0`
- check if the site is running without any problem in your browser by searching for http://`your IPv4 address`:1313

## Now to add some content:
- Create an .md file in `content/` folder. Example(index.md).

```index.md

  # Hello World


```
- in the index.md you can put your contents.


### Thats how you install and use hugo 
- if interested in hosting it in github pages checkout the official guide
[Host on Github Pages](https://gohugo.io/host-and-deploy/host-on-github-pages/)











