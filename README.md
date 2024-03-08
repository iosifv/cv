[![resume-generation](https://github.com/iosifv/cv/actions/workflows/generate-and-sync.yml/badge.svg)](https://github.com/iosifv/cv/actions/workflows/generate-and-sync.yml)
[![pages-build-deployment](https://github.com/iosifv/cv/actions/workflows/pages/pages-build-deployment/badge.svg?branch=generated-page)](https://github.com/iosifv/cv/actions/workflows/pages/pages-build-deployment)

# cv
Generator for my CV which is using the [jsonresume](https://jsonresume.org/) node app

## What's the big idea?
1. 📜 Push a change in resume.json to main branch
2. 🖥 Spin up a Github Action which:
   1. Using resume-cli generates the resume in 2 formats: html and pdf
   2. Updates the resume.json public gist
   3. Saves the generated html and pdf files to a separate branch "generated-page"
   4. A github page is synced using this branch/html
3. ☕️ Have a coffee
4. 📞 Wait for recruiters to call

## Links
- [My Gist for the json](https://gist.github.com/iosifv/bdfc617628bc7a2fc8763a2be6b1a816)
- [Generated Gihub Page](https://iosifv.github.io/cv)
- [Generated Gihub Page (pdf)](https://iosifv.github.io/cv/resume.pdf)
- [Mirrored Page on my website](https://cv.iosifv.com/)
- [Mirrored Page on my website (pdf)](https://cv.iosifv.com/resume.pdf)
  


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
- 
