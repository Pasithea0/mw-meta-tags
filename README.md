
# Movie Web / Sudo Flix Meta Tag Grabber

This will help your movie-web/sudo-flix instance grab the meta tags (movie name, movie images, movie description) to serve to preview crawlers. Such as Facebook, Discord, Telegram, Twitter, etc.

## How it works

Basically, all this script does is write out the meta tags of the movie URL. You need to setup re-write rules as movie-web/sudo-flix doesn't do server-side rendering. That's what this script is for. 

## How To Setup

This requires you to host your movie web or sudo flix instance on Cloudflare or at least use Cloudflare to proxy your DNS. 

-  Login to Cloudflare and select the domain of your main movie-web/sudo-flix site.

-  Go to Rules > Transform Rules > Modify Request Header > Create Rule 

-  Name your rule something like "If Preview Crawler"

- In the box below "Expression Preview", to the right of that box, click "Edit Expression"

- Paste the following into the box:

```
(http.user_agent contains "facebookinternalhit") or (http.user_agent contains "facebookinternalhit/") or (http.user_agent eq "facebookexternalhit/1.1 (+http://www.facebook.com/externalhit_uatext.php)") or (http.user_agent contains "TwitterBot") or (http.user_agent contains "Twitterbot") or (http.user_agent eq "Mozilla/5.0 (compatible; Discordbot/2.0; +https://discordapp.com)") or (http.user_agent contains "Discordbot/2.0") or (http.user_agent contains "Discordbot") or (http.user_agent eq "facebookexternalhit/1.1") or (http.user_agent eq "facebookcatalog/1.0") or (http.user_agent eq "Facebot/1.0") or (http.user_agent eq "SkypeUriPreview") or (http.user_agent contains "LinkedInBot/1.0") or (http.user_agent contains "facebookexternalhit/1.0") or (http.user_agent contains "Slackbot") or (http.user_agent contains "redditbot") or (http.user_agent contains "WhatsApp") or (http.user_agent contains " BingPreview") or (http.user_agent contains "Mastodon") or (http.user_agent contains "bitlybot") or (http.user_agent contains "AddThis.com")
```

- Under "Then" select "Set Static" 

- Set Header Name to `"X-Rewrite"` and Value to `"true"`

### Assuming you're using vercel to host your sudo-flix/MW instance (if not, than use .htaccess or another way to redo re-writes)

- Then go to your `vercel.json` file of your main movie-web/sudo-flix source code (not this one)

- Redo your `rewrites` to look like the following:

```
 "rewrites": [
    {
      "source": "/opensearch.xml",
      "destination": "/opensearch.xml"
    },
    {
      "source": "/media/(.*)",
      "has": [
        {
          "type": "header",
          "key": "x-rewrite",
          "value": "true"
        }
      ],
      "destination": "https://yourmetatagappdomaian.com/media/$1"
    },
    {
      "source": "/(.*)",
      "destination": "/"
    }
  ],
```

Replace https://yourmetatagappdomain.com with whatever URL you're hosting this code on. 

- Set your `TMDB_API_KEY` and `MW_BASE_URL` (MW_BASE URL = your main movie-web/sudo-flix website without trailing slash)
- Deploy your code. 

## Deploy

Click the button below to deploy to vercel. 

[![Deploy to Vercel](https://vercel.com/button)](https://vercel.com/new/joshhollys-projects/clone?repository-url=https%3A%2F%2Fgithub.com%2FPasithea0%2Fmw-meta-tags&env=TMDB_API_KEY&env=MW_BASE_URL)


## Author

[Josh Holly (wafflehacker)](https://www.github.com/joshholly)

Edits by [Pasithea](https://github.com/Pasithea0)