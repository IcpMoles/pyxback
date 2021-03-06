---
title: Staticman
author: Iacopo Moles
date: 2020-10-22T23:22:07.833+00:00
tags:
- GitHub
- setup
- configuration
- forestry
- tutorial
- comments
latex: false
toc: true
banner: "static-sites-generator.png"
math: false

---
How to setup your comments <!--more-->

## Generate Private Access Token

Head to Github and on Settings -> Developer Settings -> Personale Access Tokens and click [Generate New Token](https://github.com/settings/tokens/new).

![Github](screencapture-github-settings-tokens-new-2020-10-23-14_00_59.png "Github Token Permission")

Write a placeholder name and select the repo and user subsections. Then press Generate token.

{{< warning "Irrecoverable token">}}
Copy the new token generated and save it somewhere. Keep in mind that you can't recover it in future.
{{</ warning >}}

## Deploy on Heroku

Head over to the GitHub [project page](https://github.com/eduardoboucas/staticman/) and click the purple button (or in alternative [click here](https://dashboard.heroku.com/new?button-url=https%3A%2F%2Fgithub.com%2Feduardoboucas%2Fstaticman&template=https%3A%2F%2Fgithub.com%2Feduardoboucas%2Fstaticman)).

You will land on a page where a name for your project is asked. Type whatever name you would like and confirm.

![](heroku-landing.png)

![](building.png)

At the end of the setup you can continue on Manage App.

## Generate Private Key

From the toolbar click More -> Run Console and type `openssl genrsa -out key.pem && cat key.pem` and then press Save Session. [^linux]

![](command-pem.png)

![](save-session.png)

A .txt file is going to be downloaded.

![](txt.png)

Open it and copy the section made by:

{{< highlight http >}}
-----BEGIN RSA PRIVATE KEY-----
(a long alphanumerical text)
-----END RSA PRIVATE KEY-----
{{< / highlight>}}

You will need it later.

## Configure the environment variables

Go to Settings -> Config Vars and type the following fields:

`NODE_ENV` = `production`

`GITHUB_TOKEN` = [(Private Access Token from before)](#generate-private-access-token)

`RSA_PRIVATE_KEY` = [(Private Key from before)](#generate-private-key)

![](config-vars.png)

## Check if it works

Open a webpage and type `<name-of-your-heroku-istance>.herokuapp.com` in the urlbar. Open it and a page with `Hello from Staticman version 3.0.0!` will be shown.

Now to enable to register your comment open a webpage and type 

```http
<name-of-your-heroku-istance>.herokuapp.com/v2/connect/<github_username>/<github_repo>/main/comments
```

A confirmation will appear for only the first time.

## Configure Hugo

Open your repository with all your Hugo/Pyxill2 files and edit your `config.toml` file.

In the `[params]` section comment out (delete the hash character) the `staticman_api` variable and change the value to:

```http
https://<name-of-your-heroku-istance>.herokuapp.com/v2/entry/<github_username>/<github_repo>/main/comments
```

{{< info "Branch problems">}}
Pay attention to which branch you're submitting: `main` or `master` can cause errors if not filled correctly.
{{</ info >}}

Save.

## How to manage comments

Comments are saved in folders based on the various blog posts. You can delete or edit theme in the `/data/comments/` folder of your Hugo config. Unfortunately you need the `placeholder` folder to avoid breaking the website.

## Avatar provider

You can choose between Gravatar (faster, more widespread) or Libravatar (more privacy friendly), just set `params.avatar` to `gravatar`, `libravatar` or `avatar` if you want to use a static image (place it as `avatar.png` under the `static` directory, better if it's 100x100).


[^linux]: Yes, you can use your PC if you have the software already installed if you like, but this is more easy for a general pourpose tutorial 😊.