# Html Webpack Skip Assets Plugin
_Skip adding certain output files to the html file. Built as a drop-in replacement for [html-webpack-exclude-assets-plugin](https://www.npmjs.com/package/html-webpack-exclude-assets-plugin) and works with newer [html-webpack-plugin](https://github.com/jantimon/html-webpack-plugin) versions_

![build-plugin](https://github.com/swimmadude66/html-webpack-skip-assets-plugin/workflows/build-plugin/badge.svg?branch=master)

[![NPM](https://nodei.co/npm/html-webpack-skip-assets-plugin.png?compact=true)](https://npmjs.org/package/html-webpack-skip-assets-plugin)


## Configuration

1. Install via `npm i -D html-webpack-skip-assets-plugin`
1. Add to your webpack config AFTER HtmlWebpackPlugin
```javascript
    var HtmlWebpackSkipAssetsPlugin = require('html-webpack-skip-assets-plugin').HtmlWebpackSkipAssetsPlugin;
    // OR for import style
    import {HtmlWebpackSkipAssetsPlugin} from 'html-webpack-skip-assets-plugin'
    ...
    plugins: [
        new HtmlWebpackPlugin({
            filename: join(OUTPUT_DIR, './dist/index.html'),
            // Skip Assets options can be added here
            excludeAssets: ['polyfill.**.js', /styles\..*js$/i, (asset) => (asset.attributes && asset.attributes['x-skip'])]
            // OR
            skipAssets: ['polyfill.**.js', /styles\..*js$/i, (asset) => (asset.attributes && asset.attributes['x-skip'])]
        }),
        new HtmlWebpackSkipAssetsPlugin({
            // or they can be passed in on the plugin. These 4 lists are combined before running
            excludeAssets: ['polyfill.**.js', /styles\..*js$/i, (asset) => (asset.attributes && asset.attributes['x-skip'])]
            // OR
            skipAssets: ['polyfill.**.js', /styles\..*js$/i, (asset) => (asset.attributes && asset.attributes['x-skip'])]
        })
    ]
```

The plugin takes a configuration argument with a key called `skipAssets`. This is an array of file globs (provided via [minimatch](https://github.com/isaacs/minimatch)), regex patterns, or functions which accept the asset and return a boolean representing wheter or not to skip adding to the output html. In order to ease migration from [html-webpack-exclude-assets-plugin](https://www.npmjs.com/package/html-webpack-exclude-assets-plugin), the plugin also supports passing `excludeAssets` as the option key, as well as the ability to add either key to the HtmlWebpackPlugin options. All provided lists will be concatenated and used to filter the assets.

## Custom insertion

This exclusion will also work for `inject: false`:

```js
new HtmlWebpackPlugin({
  inject: false,
  excludeAssets: ['polyfill.**.js', /styles\..*js$/i, (asset) => (asset.attributes && asset.attributes['x-skip'])]
  templateContent: ({htmlWebpackPlugin}) => `
    <html>
      <head>
        ${htmlWebpackPlugin.tags.headTags}
      </head>
      <body>
        ${htmlWebpackPlugin.tags.bodyTags}
      </body>
    </html>
  `
})
```

## Testing
Testing is done via ts-node and mocha. Test files can be found in `/spec`, and will be auto-discovered as long as the file ends in `.spec.ts`. Just run `npm test` after installing to see the tests run.
