# @mongodb-js/compass-metrics [![][travis_img]][travis_url] [![][npm_img]][npm_url]

A [MongoDB Compass](https://github.com/mongodb-js/compass) plugin that allows you to help the team by [submitting error diagnostics and telemetry](https://docs.mongodb.com/compass/master/faq/#how-do-i-view-and-modify-my-privacy-settings). it configures [the mongodb-js-metrics module](https://github.com/mongodb-js/metrics) to handle data transport, along with a manifest of what information could be collected (see [./src/modules/rules.js](src/modules/rules.js)).

The metrics plugin is heavily inspired by [the Atom metrics package](https://atom.io/packages/metrics).

## Usage

To open the Import File dialog, a plugin emits `open-import`:

```
dispatch(globalAppRegistryEmit('open-import'));
```

[./src/modules/rules.js](src/modules/rules.js) defines what events in Compass we subscribe to for telemetry and the associated metadata we'll capture. So we can answer our question with this small rule:

```
{
  // How often is the Import Data window opened?
  registryEvent: 'open-import',
  resource: 'Import', // resource in src/modules/features.js
  action: 'opened',
  condition: () => true,
  metadata: (version) => ({
    compass_version: version
  })
}
```

## Developing

Almost all of your development will happen in the `./src` directory. Add new components
to `./src/components`, redux reducers and action creators to `./src/modules` and if you need additional
stores, add them to `./src/stores`.

To be able to debug the plugin inside `compass` make sure [webpack prod
config](./config/webpack.prod.config.js) has `devtool` is set to `source-map`.
If you want faster compiler time when you commit/push, switch it to `false.`

```js
const config = {
  target: 'electron-renderer',
  devtool: 'source-map'
};
```

#### Directory Structure

For completeness, below is a list of directories present in this module:

- `electron` code to start electron, open a browser window and load the source.
  You don't usually need to touch this, unless you want to render something other
  than the main component in Electron.
- `lib` compiled version of your components (plain javascript instead of `jsx`) and
  styles (`css` instead of `less`). Never change anything here as this entire folder
  gets automatically created and overwritten.
- `src` components, redux reducers+action creators and stores source code, as well as style files. This is the
  place to implement your own components. `npm run compile` will use `./src` as input
  and create `./lib`.
- `test` implement your tests here, and name the files `*.test.js`.

## License

Apache 2.0

[travis_img]: https://travis-ci.com/10gen/compass-metrics.svg?token=ezEB2TnpPiu7XLo6ByZp&branch=master
[travis_url]: https://travis-ci.com/10gen/compass-metrics
[npm_img]: https://img.shields.io/npm/v/@mongodb-js/compass-metrics.svg?style=flat-square
[npm_url]: https://www.npmjs.org/package/@mongodb-js/compass-metrics
