# Python uv Buildpack

A [Heroku](https://devcenter.heroku.com/) Buildpack for [uv](https://docs.astral.sh/uv/#pypi) users.

## How to use

The Python uv Buildpack runs _before_ the Heroku Python buildpack and generates the required resources for the build to be processed by a Python buildpack (like `heroku/python`). It'll generate a `requirements.txt` and `runtime.txt` from `uv.lock`. 

To set up the use of several buildpacks from the Heroku CLI use `buildpacks:add`:

```
heroku buildpacks:clear
heroku buildpacks:add
heroku buildpacks:add heroku/python
```

**Warning:** this buildpack is _only_ for generating `requirements.txt` and `runtime.txt` for subsequent use by the official `heroku/python` buildpack. If you need `uv` for other purposes, make your own separate `uv` installation.

## Configuration

### Python

Python version can be forced by setting the `PYTHON_RUNTIME_VERSION` variable. Otherwise, it will be read from `uv.lock`; for using Heroku default see below.

```
heroku config:set PYTHON_RUNTIME_VERSION=3.9.1
```

### `runtime.txt`

Generation of the `runtime.txt` can be skipped by setting `DISABLE_UV_CREATE_RUNTIME_FILE` to `1`:

```
heroku config:set DISABLE_UV_CREATE_RUNTIME_FILE=1
```

If `DISABLE_UV_CREATE_RUNTIME_FILE` is set, the required Python version can be specified in `runtime.txt`. Otherwise, if `runtime.txt` is present in the repository, the buildpack will prevent the app from being deployed in order to avoid possible ambiguities.

### uv

`uv` version can be specified by setting `UV_VERSION` in Heroku config vars. Otherwise, it will default to the latest version.

```
heroku config:set UV_VERSION=0.5.26
```

Generally all variables starting with UV_ are passed on to `uv` by this buildpack; see the corresponding [`uv` documentation](https://docs.astral.sh/uv/configuration/environment/#environment-variables) section for possible uses.

### `requirements.txt`

Exporting of development dependencies (e.g. to run tests in CI pipelines) can be optionally enabled by setting `UV_EXPORT_DEV_REQUIREMENTS` to `1`:

```
heroku config:set UV_EXPORT_DEV_REQUIREMENTS=1
```

## Credit

This buildpack is an adaptation of the [Poetry Heroku Buildpack](https://github.com/moneymeets/python-poetry-buildpack) to work with `uv`.
