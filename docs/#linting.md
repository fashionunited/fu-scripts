# Joost:

- added steps to fix markdown via cli (extra script)
- autofix in vs code on save, add this setting:
  
  "editor.codeActionsOnSave": {
    "source.fixAll.markdownlint": true
  }

## original:


My personal & professional linting setup. Extends airbnb's ESLint config first, then Prettier. Run's Prettier as an ESLint rule via their ESLint plugin. Dynamic support for react or non-react applications depending on your project dependencies. Dynamic inclusion of Kent C Dodds' ESLint Jest config depending on your project dependencies.

Convenient opt-in configs for projects using Docker or Ethereum to avoid common false positives.

To understand more, see https://github.com/codfish/eslint-config-codfish.

To avoid having to manually setup everything and add all configuration/boilerplate code to your project, use [cod-scripts](https://github.com/codfish/cod-scripts) instead. It was forked from [kcd-scripts](https://github.com/kentcdodds/kcd-scripts) and ultimately inspired by `react-scripts`.

## Why

Ultimate goal is maximize code linting coverage, leveraging the power of Prettier while deferring to Airbnb's style guide AS MUCH as possible without breaking anything. Idea being that Airbnb's styleguide happens to be the most popular, well documented, and well maintained open-source JS styleguide.

I typically work in very large teams where consensus can be difficult, and linting is like politics, people are generally passionate and rarely unopinionated. I've found that, while we all may not agree on everything in Airbnb's approach, sticking to a styleguide like theirs and avoiding opinionated overrides has had the most success across the teams I've worked on. It also helps relieve the burden of maintaining a custom set of rules (as well as the documentation).

There has to be a very good reason for an override: https://github.com/codfish/eslint-config-codfish/blob/2444e8b63426d8cea1e6da47bce1561278ddd2c2/index.js#L19

## Installation

You may be able to uninstall a number of top-level dependencies from your project but **be careful**.

```sh
npm uninstall @babel/core @babel/cli @babel/preset-env @babel/preset-react eslint eslint-config-codfish prettier husky lint-staged jest @commitlint/cli @commitlint/config-conventional markdownlint-cli
npm install --save-dev cod-scripts
npm install --save @babel/runtime
npm install -g markdownlint-cli
```

## Usage

Full usage: https://github.com/codfish/cod-scripts#usage

- Delete configuration.
```sh
rm -f .babelrc* .eslintrc* .eslintignore .huskyrc* .lintstagedrc* .prettierrc* .prettierignore jest.config.js .commitlintrc* commitlint*
```
- Delete any configs from the package.json, i.e. `jest`.
- Update `package.json` with the following:
```json
"eslintConfig": {
  "extends": [
    "./node_modules/cod-scripts/eslint.js"
  ]
},
"husky": {
  "hooks": {
    "pre-commit": "cod-scripts pre-commit",
    "commit-msg": "cod-scripts commitlint -E HUSKY_GIT_PARAMS"
  }
}
```
  - Adding `eslintConfig` to package.json is not required in theory but helps your editor enforce your eslint configuration.
- Add desired scripts to `package.json`
```json
"scripts": {
  "build": "cod-scripts build",
  "build:watch": "npm run build -- --watch",
  "format": "cod-scripts format",
  "lint": "cod-scripts lint",
  "lint:md": "markdownlint -i node_modules -i dist .",
  "lint:md:fix": "markdownlint --fix -i node_modules -i dist .",
  "test": "cod-scripts test",
  "validate": "cod-scripts validate"
}
```
