# frontend

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### Set branch gh-pages
npm install gh-pages --save-dev

### Set in package.json
"predeploy": "npm run build",
"deploy": "gh-pages -d dist"

### Set in vue.config.js > module.exports
publicPath: `/todo-list/`,

### Set commands git
git add .
git commit -m ""
git push
npm run deploy
