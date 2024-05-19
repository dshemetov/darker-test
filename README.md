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
