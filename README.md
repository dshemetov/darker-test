# darker-test

This is a repo for demoing an issue with `darker`.
It has something to do `darker` not being able to find subdirectories when they're referred to with a relative path inside a subdirectory.
It may be best to just see the example.

## Repo Structure

The directory structure is:

```
subdir/
    subsubdir/
        file.py
    Makefile
    requirements.txt
```

The `Makefile` contains a `format` target that runs `darker` on `subsubdir/`:

```makefile
REVISION ?= --revision='origin/main...'
DIFF ?=

format:
	venv/bin/darker $(DIFF) $(REVISION) subsubdir
```

The env vars allow us to use this command from CI.

The file `requirements.txt` just contains `darker~=1.7.2`:

The file `subdir/subsubdir/file.py` contains a single really long line that darker should reformat.

The repo contains four commits:

- two on the `main` branch
- one on a branch called `dev`

The first commit on `main` sets up the repo.
The second commit on `main` updates this README with the hashes of the first commit on `main` and the commit on `dev` that we want to compare against to demonstrate the issue.
The commit on `dev` modifies the single line in `subdir/subsubdir/file.py` to trigger a reformat.

## Issue Description

Note that we can run `darker` just fine on `dev` locally (against `--revision='origin/main...'`) like this:

```sh
git checkout dev
cd subdir
DIFF=--diff make format
```

and it produces expected output. However, if we run it like this:

```sh
git checkout dev
cd subdir
DIFF=--diff REVISION=--revision='' make format
```

we get the error:

```
argument PATH: Error: Path(s) 'subsubdir' do not exist in
```

This issue comes up for me in CI when using the [commit-range](https://github.com/akaihola/darker/tree/master/.github/actions/commit-range) action to run `darker` using a Makefile.
