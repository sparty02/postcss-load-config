[![npm][npm]][npm-url]
[![node][node]][node-url]
[![deps][deps]][deps-url]
[![tests][tests]][tests-url]
[![coverage][cover]][cover-url]
[![code style][style]][style-url]
[![chat][chat]][chat-url]

<div align="center">
  <img width="100" height="100" title="Load Options" src="http://michael-ciniawsky.github.io/postcss-load-options/logo.svg">
  <a href="https://github.com/postcss/postcss">
    <img width="110" height="110" title="PostCSS"           src="http://postcss.github.io/postcss/logo.svg" hspace="10">
  </a>
  <img width="100" height="100" title="Load Plugins" src="http://michael-ciniawsky.github.io/postcss-load-plugins/logo.svg">
  <h1>Load Config</h1>
  <p>Autoload Config for PostCSS<p>
</div>

<h2 align="center">Install</h2>

```bash
npm i -D postcss-load-config
```

<h2 align="center">Usage</h2>

```
npm i -S|-D postcss-plugin
```

Install plugins and save them to your ***package.json*** dependencies/devDependencies.

### `package.json`

Create **`postcss`** section in your projects **`package.json`**.

```
App
  |– client
  |– public
  |
  |- package.json
```

```json
{
  "postcss": {
    "parser": "sugarss",
    "map": false,
    "from": "/path/to/src.sss",
    "to": "/path/to/dest.css",
    "plugins": {
      "postcss-plugin": {}
    }
  }
}
```

### `.postcssrc`

Create a **`.postcssrc`** file.

```
App
  |– client
  |– public
  |
  |-.postcssrc
  |- package.json
```

```json
{
  "parser": "sugarss",
  "map": false,
  "from": "/path/to/src.sss",
  "to": "/path/to/dest.css",
  "plugins": {
    "postcss-plugin": {}
  }
}
```

### `postcss.config.js`

Create a **`postcss.config.js`** file.

```
App
  |– client
  |– public
  |
  |- postcss.config.js
  |- package.json
```

```js
module.exports = (ctx) => {
  parser: ctx.sugar ? 'sugarss' : false,
  map: ctx.env === 'development' ? ctx.map : false,
  from: ctx.from
  to: ctx.to
  plugins: {
    'postcss-plugin': ctx.plugin
  }
}
```

```js
module.exports = (ctx) => {
  parser: ctx.sugar ? 'sugarss' : false,
  map: ctx.env === 'development' ? ctx.map : false,
  from: ctx.from
  to: ctx.to
  plugins: [
    require('postcss-plugin')(ctx.plugin)
  ]
}
```

<h2 align="center">Options</h2>

Plugin **options** can take the following values.



**`null`**: Plugin loads with defaults.

```js
'postcss-plugin': null
```

**`[Object]`**: Plugin loads with given options.

```js
'postcss-plugin': { option: '', option: '' }
```

**`false`**: Plugin will not be loaded.

```js
'postcss-plugin': false
```

### Order

Plugin **order** is determined by declaration in the plugins section.

```js
{
  plugins: {
    'postcss-plugin': false, // plugins[0]
    'postcss-plugin': false, // plugins[1]
    'postcss-plugin': {}     // plugins[2]
  }
}
```

### Context

When using a function `(postcss.config.js)`, it is possible to pass context to `postcss-load-plugins`, which will be evaluated before loading your plugins. By default `ctx.env (process.env.NODE_ENV)` and `ctx.cwd (process.cwd())` are available.

<h2 align="center">Examples</h2>

**postcss.config.js**

```js
module.exports = (ctx) => {
  parser: ctx.sugar ? 'sugarss' : false,
  map: ctx.env === 'development' ? ctx.map : false,
  from: ctx.from
  to: ctx.to
  plugins: {
    postcss-import: null,
    postcss-nested: null,
    cssnano: ctx.env === 'development' ? false : null
  }
}
```

### <img width="80" height="80" src="https://worldvectorlogo.com/logos/nodejs-icon.svg">

```json
"scripts": {
  "build": "NODE_ENV=production node postcss",
  "start": "NODE_ENV=development node postcss"
}
```

```js
const { readFileSync } = require('fs')

const postcss = require('postcss')
const postcssrc = require('postcss-load-config')

const css = readFileSync('index.sss', 'utf8')

const ctx = { map: 'inline' from: '/index.sss', to: '/index.css' }

postcssrc(ctx).then(({ plugins, options }) => {
  postcss(plugins)
    .process(css, options)
    .then(({ css }) => console.log(css))
}))
```

### <img width="80" height="80" src="https://worldvectorlogo.com/logos/gulp.svg">

```json
"scripts": {
  "build": "NODE_ENV=production gulp",
  "start": "NODE_ENV=development gulp"
}
```

```js
const { task, src, dest } = require('gulp')

const postcss = require('gulp-postcssrc')

task('css', () => {
  src('src/*.css')
    .pipe(postcss())
    .pipe(dest('dest'))
})

task('default', ['css'])
```

### <img width="80" height="80" src="https://worldvectorlogo.com/logos/webpack.svg">

```json
"scripts": {
  "build": "NODE_ENV=production webpack",
  "start": "NODE_ENV=development webpack-dev-server"
}
```

```js
module.exports = (env) => {
  module: {
    rules: [
      {
        test: /\.css$/
        use: [
          'style-loader',
          {
            loader: 'css-loader',
            options: { importLoaders: 1 } }
          },
          'postcss-loader'
        ]
      }
    ]
  }
}
```

<h2 align="center">Maintainers</h2>

<table>
  <tbody>
   <tr>
    <td align="center">
      <img width="150 height="150"
      src="https://avatars.githubusercontent.com/u/5419992?v=3&s=150">
      <br />
      <a href="https://github.com/michael-ciniawsky">Michael Ciniawsky</a>
    </td>
    <td align="center">
      <img width="150 height="150"
      src="https://avatars.githubusercontent.com/u/2437969?v=3&s=150">
      <br />
      <a href="https://github.com/ertrzyiks">Mateusz Derks</a>
    </td>
  </tr>
  <tbody>
</table>


[npm]: https://img.shields.io/npm/v/postcss-load-config.svg
[npm-url]: https://npmjs.com/package/postcss-load-config

[node]: https://img.shields.io/node/v/postcss-load-plugins.svg
[node-url]: https://nodejs.org/

[deps]: https://david-dm.org/michael-ciniawsky/postcss-load-config.svg
[deps-url]: https://david-dm.org/michael-ciniawsky/postcss-load-config

[style]: https://img.shields.io/badge/code%20style-standard-yellow.svg
[style-url]: http://standardjs.com/

[tests]: http://img.shields.io/travis/michael-ciniawsky/postcss-load-config.svg?branch=master
[tests-url]: https://travis-ci.org/michael-ciniawsky/postcss-load-config?branch=master

[cover]: https://coveralls.io/repos/github/michael-ciniawsky/postcss-load-config/badge.svg?branch=master
[cover-url]: https://coveralls.io/github/michael-ciniawsky/postcss-load-config?branch=master

[chat]: https://img.shields.io/gitter/room/postcss/postcss.svg?maxAge=2592000
[chat-url]: https://gitter.im/postcss/postcss
