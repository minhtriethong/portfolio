---
title: "Create a static website with Hugo and deploy to Github"
date: 2022-08-14T20:25:05+09:00
Description: ""
Tags: []
Categories: []
DisableComments: false
---
This protocol is performed on window 10, and require to install these package/tool
1. Start menu --> Type `cmd` --> Run as administrator
2. Create a new github account --> `github-id`
3. Install [github for window](https://git-scm.com/download/win)
4. Install Chocolatey --> Copy the following command to cmd --> Enter
```
@"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "[System.Net.ServicePointManager]::SecurityProtocol = 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"
```
---
1. Start menu --> Type `cmd` --> Run as administrator
2. Change to directory for your website --> `cd websitedir`
3. Type to CMD (1 line/time --> enter)
```
choco install hugo -confirm
hugo new site sitename
cd websitedir
git init
git status
git add .
git commit -m "first commit"
```
4. Choose a [HUGO theme](https://themes.gohugo.io/) --> get the github link of the theme (Ex: Choose theme *anatole* --> https://github.com/lxndrblz/anatole)
5. Type to CMD (1 line/time --> enter)
```
git submodule add https://github.com/lxndrblz/anatole themes/anatole
```
6. Create a new github repository
```
Name: sitename
Public
```
7. Update file **config.toml**
```
baseURL = "https://github-id.github.io/"
languageCode = "en-us"
title = "sitename"
theme = "anatole"
```
8. Type to CMD (1 line/time --> enter)
```
git status
git add .
git commit -m "ready to upload"
git branch -M main
git remote add origin https://github.com/github-id/sitename.git
git push -u origin main
```
9. Create folder and file in root `.github/workflows/gh-pages.yml`
10. Go to this [HUGO website](https://gohugo.io/hosting-and-deployment/hosting-on-github/#build-hugo-with-github-action) --> Build Hugo With GitHub Action --> Copy the following script to the file `gh-pages.yml`
```
name: github pages

on:
  push:
    branches:
      - main  # Set a branch to deploy
  pull_request:

jobs:
  deploy:
    runs-on: ubuntu-20.04
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        if: github.ref == 'refs/heads/main'
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```
11. Type to CMD (1 line/time --> enter)
```
git status
git add .
git commit -m "deploy"
git push
```
12. Go to the newly created github repository (sitename)
13. Go to tab:Setting --> Pages --> Source --> Select branch: gh-pages --> Save
14. Wait for 2 minutes --> Website is live at this URL `https://github-id.github.io/`