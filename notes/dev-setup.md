---
title: Dev Project Setup
tags:
  - other
emoji: 📔
---

## Packages

### Commitizen and Husky

When you commit with Commitizen, you'll be prompted to fill out any required commit fields at commit time. An adapter (cz-conventional-changelog) will tell Commitizen what format to use.

```cli
npx commitizen init cz-conventional-changelog --save-dev --save-exact
```

To run Commitizen on git commit command (otherwise, use git cz), will need to use Husky to run before making a commit. Husky listen to git hooks to trigger certain actions.

Note, instead of adding husky hooks to package.json, we will add them to .husky folder according to each git hook.

```cli
# install husky
npm install husky --save-dev
# enable Git hooks
npx husky install
# to automatically have Git hooks enabled after install, edit package.json
npm set-script prepare "husky install"
# note, yarn doesn't support prepare, need to have different install, see husky docs

# install Commitizen hook, make sure to run husky install
npx husky add .husky/prepare-commit-msg 'exec < /dev/tty && git cz --hook || true'
```

### Commitlint

commitlint check if commit messages meet the conventional commit format. commitlint CLI is a primary way to interact with commitlint.

```cli
npm install --save-dev @commitlint/{cli,config-conventional}
# add husky hook to run on commit-msg
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit $1'
```

### Lint-staged

Lint-staged is specifically made to as the name implies, lint staged code before it gets committed.

This command will install and configure husky and lint-staged depending on the code quality tools from your project's package.json dependencies, so please make sure you install (npm install --save-dev) and configure all code quality tools like Prettier and ESLint prior to that.

```cli
npx mrm@2 lint-staged
# add husky hook
npx husky add .husky/pre-commit 'yearn lint-staged'
```

Then, in package.json add (or specify other options, see docs):
```js
"scripts": {
    "lint": "eslint --ignore-path .gitignore \"src/**/*.+(ts|js|tsx)\"",
    "format": "prettier --ignore-path .gitignore \"src/**/*.+(ts|js|tsx)\" --write",
},
"lint-staged": {
	"./src/**/*.{ts,js,jsx,tsx}": [
		"yarn lint --fix",
		"yarn format"
	]
}
```



