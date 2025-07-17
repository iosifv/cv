[![resume-generation](https://github.com/iosifv/cv/actions/workflows/generate-and-sync.yml/badge.svg)](https://github.com/iosifv/cv/actions/workflows/generate-and-sync.yml)
[![pages-build-deployment](https://github.com/iosifv/cv/actions/workflows/pages/pages-build-deployment/badge.svg?branch=generated-page)](https://github.com/iosifv/cv/actions/workflows/pages/pages-build-deployment)

# cv
Generator for my CV which is using the [jsonresume](https://jsonresume.org/) node app

## What's the big idea?
1. 📜 Push a change in resume.json to main branch
2. 🖥 Spin up a Github Action which:
   1. Using resume-cli generates the resume in 2 formats: html and pdf
   2. Updates the resume.json public gist
   3. Saves the generated html and pdf files to a separate branch: "generated-page"
   4. A github page is synced using this branch/html
3. ☕️ Have a coffee

## Links (sitemap)
- [Github - Associated Gist for this page](https://gist.github.com/iosifv/bdfc617628bc7a2fc8763a2be6b1a816)
- [Github - Generated Gihub Page - LinkHub](https://iosifv.github.io/cv)
- [Github - Generated Gihub Page - generated cv.html (view.html)](https://iosifv.github.io/cv/view.html)
- [Github - Generated Gihub Page - generated cv.pdf](https://iosifv.github.io/cv/cv.pdf)
- [Github - Generated Gihub Page - Iosif_Vigh-Senior_Software_Engineer.pdf](https://iosifv.github.io/cv/Iosif_Vigh-Senior_Software_Engineer.pdf)
- [cv.iosifv.com - LinkHub](https://cv.iosifv.com/)
- [cv.iosifv.com - View online CV](https://cv.iosifv.com/view)
- [cv.iosifv.com - cv.pdf](https://cv.iosifv.com/cv.pdf)
- [cv.iosifv.com - Iosif_Vigh-Senior_Software_Engineer.pdf](https://cv.iosifv.com/Iosif_Vigh-Senior_Software_Engineer.pdf)
  

####  Jsonresume Registry (generated from gist)
- [theme=caffeine](https://registry.jsonresume.org/iosifv?theme=caffeine)
- [theme=stackoverflow](https://registry.jsonresume.org/iosifv?theme=stackoverflow)

#### Actions used
- [rvdwegen/action-jsonresume-convert](https://github.com/marketplace/actions/jsonresume-convert)
- [exuanbo/actions-deploy-gist](https://github.com/marketplace/actions/deploy-to-gist)
- [ad-m/github-push-action](https://github.com/ad-m/github-push-action)

#### Documentation and useful links
- [JSON Schema - resume.sample](https://github.com/jsonresume/resume-schema/blob/master/sample.resume.json)
- [JSON Schema - oficial schema](https://github.com/jsonresume/resume-schema/blob/master/schema.json)
- [JSON Schema - thomasdavis](https://gist.github.com/thomasdavis/c9dcfa1b37dec07fb2ee7f36d7278105)
- [Theme - Stackoverflow](https://www.npmjs.com/package/jsonresume-theme-stackoverflow)
- [Icon list for the network tag](https://fontawesome.com/search?o=r&ip=brands)


## Troubleshooting
- If you get an error like this:
```
Run ad-m/github-push-action@master
Push to branch generated-page
remote: Invalid username or password.
fatal: Authentication failed for 'https://github.com/iosifv/cv.git/'
Error: Invalid exit code: 128
    at ChildProcess.<anonymous> (/home/runner/work/_actions/ad-m/github-push-action/master/start.js:30:21)
    at ChildProcess.emit (node:events:519:28)
    at maybeClose (node:internal/child_process:1105:16)
    at ChildProcess._handle.onexit (node:internal/child_process:305:5) {
  code: 128
}
```
Just update the `GHA_TOKEN_FOR_PUSH_EXPIRES_FEB_2025` token in the github settings.

- Another one: The github action used for this is somehow forcing the last package to be used. Version 2.1.0 of the stackoverflow theme is not showing the company names so the package.json version must remain `2.0`