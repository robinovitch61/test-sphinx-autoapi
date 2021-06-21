## Strange bug...

This is a bug report for a strange bug with sphinx-autoapi.

When I run `sphinx-build` with a `-c` option to specify the `conf.py` location where the `conf.py` file is in a different location than the docs index, I get the following extension error:
```
dumping object inventory... done
build succeeded, 1 warning.

The HTML pages are in gen.

Extension error (autoapi.extension):
Handler <function build_finished at 0x10a2eae50> for event 'build-finished' threw an exception (exception: [Errno 2] No such file or directory: '/Users/leo/projects/test-autoapi/autoapi')
```

A new directory remains after running `sphinx-build` full of `autoapi` files that I believe should be removed.

### Steps to Reproduce:
Python 3.8.2, MAC OS Big Sur, installed requirements.txt.

Happy path (`working`) branch of this directory:
```
.
├── code
│   └── test.py
├── docs
│   ├── conf.py
│   └── index.rst
├── gen
└── requirements.txt
```
Run:
```
# cd to root of this repo on `working` branch
rm -rf ./gen/*
sphinx-build -b html ./docs -c ./docs ./gen
python -m http.server --directory ./gen 8000
```

Visit http://localhost:8000, see contents.

"Bug" path (`not-working` branch of this directory:
```
.
├── code
│   └── test.py
├── conf.py
├── docs
│   └── index.rst
├── gen
└── requirements.txt
```
Run:
```
# cd to root of this repo on `not-working` branch
rm -rf ./gen/*
sphinx-build -b html ./docs -c . ./gen
python -m http.server --directory ./gen 8000
```

See error as described. Visit http://localhost:8000, see contents - looks ok.

But run:
```
ls docs
```

and see how `autoapi` directory remains. I suspect this is related to the error.

