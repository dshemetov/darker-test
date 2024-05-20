REVISION ?= --revision='origin/main...'
DIFF ?=

uv:
	curl -LsSf https://astral.sh/uv/install.sh | sh

venv: uv
	uv venv venv

install: venv
	. venv/bin/activate; uv pip install -r requirements.txt

format:
	venv/bin/darker $(DIFF) $(REVISION) subsubdir
