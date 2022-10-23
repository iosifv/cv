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
  


####  Jsonresume Registry (generated from gist)
- [theme=caffeine](https://registry.jsonresume.org/iosifv?theme=caffeine)
- [theme=stackoverflow](https://registry.jsonresume.org/iosifv?theme=stackoverflow)

#### Actions used
- [rvdwegen/action-jsonresume-convert](https://github.com/marketplace/actions/jsonresume-convert)
- [exuanbo/actions-deploy-gist](https://github.com/marketplace/actions/deploy-to-gist)
- [ad-m/github-push-action](https://github.com/ad-m/github-push-action)

