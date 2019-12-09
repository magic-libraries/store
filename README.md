## @magic-libraries/db

localstorage backed client key value database for
[@magic](https://magic.github.io/core)

[html-docs](https://magic-libraries.github.io/db)

[![NPM version][npm-image]][npm-url]
[![Linux Build Status][travis-image]][travis-url]
[![Windows Build Status][appveyor-image]][appveyor-url]
[![Coverage Status][coveralls-image]][coveralls-url]
[![Greenkeeper badge][greenkeeper-image]][greenkeeper-url]
[![Known Vulnerabilities][snyk-image]][snyk-url]

#### <a name="install"></a>installation
```bash
npm install --save-exact @magic-libraries/db
```

#### <a name="usage"></a>usage
in a page/component, just use the lib.db functions, either as action or effect.

localstorage is synchronous, so we do not have to await.

see [ExampleStore](https://github.com/magic-libraries/db/tree/master/example/assets/ExampleStore) for a reference implementation,

and [@magic-modules/gdpr](https://github.com/magic-modules/gdpr) for an actual usecase.

```javascript
export const View = ({ key, state }) =>
  div({ onload: state => console.log('load') || state }, [
    div(['key: ', key]),
    div([
      h4('controls'),
      button({ onclick: [actions.es.write, { key }] }, 'write'),
      button({ onclick: [actions.es.read, { key }] }, 'read'),
      button({ onclick: [actions.es.clear, { key }] }, 'clear'),
    ]),
    div('value in local storage:'),
    state[key]
      ? [`state is accessible via state['${key}']`, div(state[key])]
      : div('no value in db'),
  ])

export const actions = {
  es: {
    read: (state, { key }) => [
      state,
      [
        lib.db.read,
        {
          key,
          action: actions.examplestore.refresh,
        },
      ],
    ],

    write: (state, { key }) => [
      state,
      [
        lib.db.write,
        {
          key,
          value: `testing ${Math.ceil(Math.random() * 100000)}`,
          action: actions.examplestore.refresh,
        },
      ],
    ],

    clear: (state, { key }) => [
      state,
      [
        lib.db.clear,
        {
          key,
          action: actions.examplestore.refresh,
        }
      ],
    ],

    refresh: (state, { key, value }) => {
      console.log('refresh', { key, value })

      if (key) {
        state[key] = value
      }

      return {
        ...state,
      }
    },
  },
}
```

#### changelog
##### 0.0.1
first release

[npm-image]: https://img.shields.io/npm/v/@magic-libraries/db.svg
[npm-url]: https://www.npmjs.com/package/@magic-libraries/db
[travis-image]: https://img.shields.io/travis/com/magic-libraries/db/master
[travis-url]: https://travis-ci.com/magic-libraries/db
[appveyor-image]: https://img.shields.io/appveyor/ci/magiclibraries/db/master.svg
[appveyor-url]: https://ci.appveyor.com/project/magiclibraries/db/branch/master
[coveralls-image]: https://coveralls.io/repos/github/magic-libraries/db/badge.svg
[coveralls-url]: https://coveralls.io/github/magic-libraries/db
[greenkeeper-image]: https://badges.greenkeeper.io/magic-libraries/db.svg
[greenkeeper-url]: https://badges.greenkeeper.io/magic-libraries/db.svg
[snyk-image]: https://snyk.io/test/github/magic-libraries/db/badge.svg
[snyk-url]: https://snyk.io/test/github/magic-libraries/db
