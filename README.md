
# Movie-web / Sudo-flix Meta Tag Grabber

This will help your movie-web/sudo-flix instance grab the meta tags (movie name, movie images, movie description) to serve to preview crawlers, such as Facebook, Discord, Telegram, Twitter, etc.

## How it works

If the page is crawled by a bot, for example, a Discord embed, the script receives the original watch page URL, extracts the TMDB ID, and searches for the metadata. Then it hosts a new page only visible to bots with the metadata.

## How To Setup

This requires you to host your movie-web or sudo-flix instance on Cloudflare or at least use Cloudflare to proxy your DNS. 

### 1. Setup this project

Understanding the environment variables:

- `TMDB_API_KEY` this is not the same as the TMDB READ API KEY. It should look something like: a50e561sa68582035jb0e3e36c0049f

- `MW_BASE_URL` your main movie-web/sudo-flix website without trailing slash. Ex: https://yoursite.example

- `MW_TITLE` this is the name of your movie-web/sudo-flix instance. Ex: "Sudo-flix". This will look something like: `Dune: Part Two | Sudo-flix - Stream Movies & TV Shows For Free` in the meta tags.

-  Click the button below to deploy to vercel. 

[![Deploy to Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https%3A%2F%2Fgithub.com%2FPasithea0%2Fmw-meta-tags&env=TMDB_API_KEY&env=MW_BASE_URL&env=MW_TITLE)

### 2. Setup Cloudflare

Note: This will not work with WAF. You must define a skip-rule so crawlers can load the page first.

-  Login to Cloudflare and select the domain of your main movie-web/sudo-flix site.

-  Go to Rules, and create a new "Redirect Rule".

-  Name your rule something like "Meta Redirect"

-  Select "Custom filter expression"

-  In the box below "Expression Preview", to the right of that box, click "Edit Expression"

-  Paste the following into the box:

```
((http.user_agent contains "facebookinternalhit") or 
 (http.user_agent contains "facebookinternalhit/") or 
 (http.user_agent eq "facebookexternalhit/1.1 (+http://www.facebook.com/externalhit_uatext.php)") or 
 (http.user_agent contains "TwitterBot") or 
 (http.user_agent contains "Twitterbot") or 
 (http.user_agent eq "Mozilla/5.0 (compatible; Discordbot/2.0; +https://discordapp.com)") or 
 (http.user_agent contains "Discordbot/2.0") or 
 (http.user_agent contains "Discordbot") or 
 (http.user_agent eq "facebookexternalhit/1.1") or 
 (http.user_agent eq "facebookcatalog/1.0") or 
 (http.user_agent eq "Facebot/1.0") or 
 (http.user_agent eq "SkypeUriPreview") or 
 (http.user_agent contains "LinkedInBot/1.0") or 
 (http.user_agent contains "facebookexternalhit/1.0") or 
 (http.user_agent contains "Slackbot") or 
 (http.user_agent contains "redditbot") or 
 (http.user_agent contains "WhatsApp") or 
 (http.user_agent contains " BingPreview") or 
 (http.user_agent contains "Mastodon") or 
 (http.user_agent contains "bitlybot") or 
 (http.user_agent contains "AddThis.com")) and 
 (http.request.full_uri wildcard "https://yoursite.example/media/*")
```

-  Replace "yoursite.example" on the last line with your main movie-web/sudo-flix domain.

-  Then set the type to Dynamic and 301, then paste the following in the box:

```
wildcard_replace(http.request.full_uri, r"https://yoursite.example/media/*", r"https://thismetahost.site/media/${1}")
```

-  Replace "yoursite.example" with your main movie-web/sudo-flix domain, and replace "thismetahost.site" with the domain this project is hosted on.

-  Then deploy the rule.


## Author

[Josh Holly (wafflehacker)](https://www.github.com/joshholly)

Edits by [Pasithea](https://github.com/Pasithea0)