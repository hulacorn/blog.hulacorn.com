### To get starteed, run the following commands:

```
npm install
npm run serve
```

### To deploy the site

```
git add .
git commit -m "Update"
git tag -a 0.0.x
git push --tags
```

### Confirm changes have been deployed

Go to the actions tab on the github repo, [blog.hulacorn.com](https://github.com/padraigobrien/blog.hulacorn.com)
if you tagged correctly then it should build and deploy to S3 automatically.
