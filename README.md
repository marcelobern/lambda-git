# lambda-git
[![NPM Version](https://img.shields.io/npm/v/lambda-git.svg)](https://www.npmjs.com/package/lambda-git)
[![Build Status](https://travis-ci.org/pimterry/lambda-git.svg?branch=master)](https://travis-ci.org/pimterry/lambda-git)
[![Coverage Status](https://coveralls.io/repos/github/pimterry/lambda-git/badge.svg?branch=master)](https://coveralls.io/github/pimterry/lambda-git?branch=master)

A git binary installed through NPM, for use with AWS Lambda.

To use this, just require it, and call it. E.g:

```javascript
require("lambda-git")();
```

This call returns a promise, and once it completes your Node process will be set up to run Git as a subprocess. This extracts a lambda-built version of Git, updates the required environment variables to make it functional, and updates your process.env.PATH to make it accessible.

A larger example:

```javascript
const { exec } = require('child_process');

require("lambda-git")()
	.then(function () {
		// git is now ready
    exec("git --version");
	})
	.catch(function (error) {
		// something failed
	});
```

If you want to do something more complicated, you can provide options to change this behaviour.

The git binary itself comes from [LambCI](https://github.com/lambci/lambci/tree/version-0.10.0/vendor).

## Changing the installation path:

```javascript
require("lambda-git")({
  targetDirectory: "/tmp/alternate/path/git" // no action taken if this directory exists
});
```

## Managing env vars yourself:

```javascript
require("lambda-git")({
  updateEnv: false
});
/* Returns:
{
  binPath: "/tmp/git/usr/bin",
  env: {
    GIT_TEMPLATE_DIR: '/tmp/git/usr/share/git-core/templates',
    GIT_EXEC_PATH: '/tmp/git/usr/libexec/git-core'
  }
} */
```

You'll need to extract these values and ensure they're made available to whatever process you're using to run Git.
