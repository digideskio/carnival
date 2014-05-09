# Carnival

A medium/disqus-like commenting service powering http://robots.thoughtbot.com.

## Getting started

Install ghc, alex, happy, cabal, Postgresql. On OS X, the easiest way is to
install Haskell Platform using homebrew:

    $ brew install haskell-platform

Run the setup script first:

    $ bin/setup

Then boot the site:

    $ yesod devel

And run the tests:

    $ yesod test

## Deployment

At this time, we're unable to come in under the 15 minute build timeout 
on Heroku, therefore we need to deploy via an Anvil server:

```
$ heroku plugins:install https://github.com/ddollar/heroku-anvil
```

### Staging

```
$ heroku build -r -b https://github.com/begriffs/heroku-buildpack-ghc.git -a carnival-staging
```

### Production

```
$ heroku build -r -b https://github.com/begriffs/heroku-buildpack-ghc.git -a carnival-production
```

## Managing Dependencies

The `bin/setup` script will create a [cabal sandbox][cabal-sandbox] and 
install dependencies into it. If new dependencies are added to the 
project, they'll need to be installed via:

[cabal-sandbox]: http://coldwa.st/e/blog/2013-08-20-Cabal-sandbox.html

```
$ cabal install --dependencies-only
```

The `bin/setup` script is idempotent, so you can simply re-run that to 
install all of the dependencies for you if you prefer.

It is entirely possible that the introduction of a new dependency 
requires upgrades of existing dependencies. This situation can be 
difficult to manage depending on the exact circumstances and may result 
in "cabal hell".

Thanks to the introduction of sandboxes, one can always reset from 
scratch:

```
$ cabal sandbox delete
$ cabal sandbox init
$ cabal update
$ cabal install --dependencies-only
```

This process should always work as long as there is a valid set of 
dependency versions available.

This process can also be done for you by setting the `RESET` environment 
variable and (re-)running `bin/setup`:

```
$ RESET=1 ./bin/setup
```

This may be made easier in the future by using [cabal-constraints][cabal-constraints].

[cabal-constraints]: https://github.com/thoughtbot/carnival/issues/5
