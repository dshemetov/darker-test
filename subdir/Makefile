REVISION ?= --revision='origin/main...'
DIFF ?=

venv:
	python -m venv venv

install: venv
	venv/bin/pip install -r requirements.txt

format:
	venv/bin/darker $(DIFF) $(REVISION) subsubdir
