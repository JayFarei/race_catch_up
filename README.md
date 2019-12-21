## Static website using Hugo

This repository is a static website generated via Hugo, customised in atom, content generated in markdown in Atom, committed to github via Atom, redirected to a custom domain via Github pages.

This page will take you through all of the steps required to have a live website on your custom domain.


### Step by step instructions

1. Install git

```
brew install git
```

2. Install [Atom](https://atom.io)

```
brew cask install atom
```

3. Install [Hugo](https://gohugo.io/getting-started/quick-start/)

```
brew install hugo

```

4. Check the version of hugo

```
hugo version

```

5. Obtain a custom domain on Godaddy

Go to [godaddy.com](godaddy.com) - find a domain you like and purchase it - leave the tab open as you'll need to configure the DNS at a latest step

6. Create a new site

Navigate to your development folder

```
cd ~/Dropbox/development/
```
Create the base folder for hugo
```
hugo new site quickstart
```

7. Rename your project folder

```
rmdir quickstart Static_Website
```
8. Find a theme

In this case I am using [Terminal_theme](https://github.com/panr/hugo-theme-terminal.git)

9. Navigate to your Hugo project

```
cd ~/dropbox/development/Static_Website
```

10. Clone the repo in your theme folder

```
git clone https://github.com/panr/hugo-theme-terminal.git themes/terminal
```
11. Open the project folder in Atom

Open Atom and load the project folder
`~/Dropbox/development/Static_Website`

12. Configure your hugo base

Within Atom, navigate to `~/Dropbox/development/Static_Website` locate `config.toml` and copy the following configuration

Ps. adjust the `baseurl` your godaddy custom domain

```
baseurl = "<you godaddy custom domain>"
languageCode = "en-us"
theme = "terminal"
paginate = 5

[params]
  # dir name of your blog content (default is `content/posts`)
  contentTypeName = "posts"
  # ["orange", "blue", "red", "green", "pink"]
  themeColor = "orange"
  # if you set this to 0, only submenu trigger will be visible
  showMenuItems = 2
  # show selector to switch language
  showLanguageSelector = false
  # set theme to full screen width
  fullWidthTheme = false
  # center theme with default width
  centerTheme = false
  # set a custom favicon (default is a `themeColor` square)
  # favicon = "favicon.ico"

[languages]
  [languages.en]
    languageName = "English"
    title = "Terminal"
    subtitle = "A simple, retro theme for Hugo"
    keywords = ""
    copyright = ""
    menuMore = "Show more"
    readMore = "Read more"
    readOtherPosts = "Read other posts"

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
```
Save with `Cmd+S`

13. Set up your content layout folder

Within `themes/terminal/exampleSite` you'll find a typical layout of the content expected by this template. You'll see that posts are simple markdown pages. That there is a folder name `post` --> go ahead and rename that `posts` to match your config. Then copy the entire content of the folder under `~/Dropbox/development/Static_Website/content`

14. Test your set up

You should now have a viable hugo static site.

Navigate with your terminal to your hugo base folder

```
cd ~/dropbox/development/Static_Website
```
And start a local hugo server

```
hugo server -D
```
Your site is now available locally at [http://localhost:1313/](http://localhost:1313/)

Now any change you make in Atom to config or `.md` posts will be reflected live in your local server

15. Set up your repo in Atom

* Follow the instructions you see inside Atom’s GitHub tab. First, visit the [github.atom.io/](github.atom.io/) login URL and sign in to your GitHub account. Here, you can generate a token with which you can perform the authorization.

* Go to [github.com](github.com) - create a new repository (make it public)

* Open *Atom* - type `cmd+shift+P` and type *github clone*

* Select as destination your development folder (not the same folder as your hugo base) - in my case is `~/dropbox/development`

* Copy and past the git URL from your repository `https://github.com/jayfarei/RCU.git`

* You should now have an empty repository on Atom (as you have in Github.com)

16. Export your hugo site and import it in the repository

Navigate to your hugo base folder

```
cd ~/dropbox/development/Static_Website
```
Once you are done drafting the content, delete the contents of public folder and run
```
hugo
```
to regenerate the files in your hugo base public folder.

You should have something like this:

```
| EN
+------------------+----+
Pages            |  9
Paginator pages  |  0
Non-page files   |  0
Static files     | 17
Processed images |  0
Aliases          |  4
Sitemaps         |  1
Cleaned          |  0

Total in 48 ms
```

Now the only thing left is to commit those files into your github repository

You can do it manually - via terminal you can use the following command

```
cp -a ~/dropbox/development/Static_Website/public/. ~/dropbox/development/RCU
```

17. Commit your change

By opening the `git` tab you should see a lot of uncommitted changes - stage them / comment your commit and confirm.
Finally - press Push.

Note: You can select the branch in which you want to push your commit. If not - make sure that the branch is merged into master before the next step.

18. Set up Github pages

* Go to your repository [github.com](github.com) - go to settings - scroll down to `Github Pages` - enter your custom domain - in my case is `racecatchup.com`
> By doing so - Github will generate a file called `CNAME` in your master branch with the custom domain in it.

* Go back to [godaddy.com](godaddy.com) - go to manage your domain - DNM - and replicate the following set up:

Type  | Hosts  | Points to          | TTL    | Seconds  |
------|:------:|:------------------:| :-----:|-------:  |
A     | @      | 185.199.108.153    | custom | 600s     |
A     | @      | 185.199.109.153    | custom | 600s     |
A     | @      | 185.199.110.153    | custom | 600s     |
A     | @      | 185.199.111.153    | custom | 600s     |
CNAME | www    | jayfarei.github.io | 1h     | N/A      |

Delete any other records, leave Godaddy name servers.

Verify your set up by checking with your terminal:

```
dig <custom_domain>.com
```
The outcome should confirm the following:

```
; ANSWER SECTION:
racecatchup.com.	600	IN	A	185.199.111.153
racecatchup.com.	600	IN	A	185.199.108.153
racecatchup.com.	600	IN	A	185.199.109.153
racecatchup.com.	600	IN	A	185.199.110.153
```
It will take at least 30min to propagate, once it does - if you navigate to your domain you'll realise it misses HTTPS and does not render well or doesn't render at all.

18. Enable HTTPS
Go back to Github pages - and select the option `Enforce HTTPS`


19. That is done

Now your site should be up and running, every time you merge a branch to your master - you are deploying a new version of the website
