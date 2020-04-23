---
type: quick-start
---

# Configure environment

To preview our new guide, we will want to run a minimal version of the Box
developer docs. We cloned the `developer.box.com-framework` repository in the
previous step. This project is pretty much ready to go, yet we need to do a few
things first.

## Install dependencies

First change your terminal into the directory, and then install the dependencies
using Yarn.

```bash
cd developer.box.com-framework
yarn install
```

<Message>You can install Yarn as a global package manager using `npm i -g
yarn`</Message>

## Configure environment variables

Next, copy the `.env.example` file to a new file called `.env`.

```bash
cp .env.example .env
```

This creates a new `.env` file that won't be committed to GitHub. You can make
changes to this to your liking for development purposes.

For the purpose of creating a guide locally on your machine, you will want to
make a few small adjustments to the defaults.

Firstly, you will want to set the feature flips to only build pages and guides,
and not the API explorer.

```ini;highlight=1-2
GATSBY_FEATURE_PAGES=true
GATSBY_FEATURE_GUIDES=true
GATSBY_FEATURE_REFERENCE=false
GATSBY_FEATURE_EXPLORER=false
GATSBY_FEATURE_ANALYTICS=false
GATSBY_FEATURES_LINTING=false
GATSBY_ENABLE_BUILD_HOOKS=false
```

Secondly, you will want to comment out the `CONTENT_REPO` variable, and instead
point the `CONTENT_PATH` at your `developer.box.com`'s `compiled` folder that
you checked out from GitHub in step 1.

```ini
# CONTENT_REPO="https://github.com/box/developer.box.com.git#en"
CONTENT_PATH='/path/to/developer.box.com/compiled'
```

<Next>I've configured the environment</Next>
