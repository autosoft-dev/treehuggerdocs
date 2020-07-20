---
title: Treehugger Docs

---

## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/autosoft-dev/treehuggerdocs/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.


## Browse the documentation


### Installation and setup

### From pip:

Just do
```
pip install tree-hugger
```

### From Source:

```
git clone https://github.com/autosoft-dev/tree-hugger.git

cd tree-hugger

pip install -e .
```

_The installation process is tested in macOS Mojave, we have a [separate docker binding](https://github.com/autosoft-dev/tree-sitter-docker) for compiling the libraries for Linux and soon this library will be integrated in that as well_

_You may need to install libgit2. In case you are in mac just use `brew install libgit2`_

## Building the .so files

_Please note that building the libraries has been tested under a macOS Mojave with Apple LLVM version 10.0.1 (clang-1001.0.46.4)_

_Please check out our Linux specific instructions [here](https://github.com/autosoft-dev/tree-sitter-docker)_

Once this library is installed it gives you a command line utility to download and compile tree-sitter .so files with ease. As an example - 

```
create_libs python
```

Here is the full usage guide of the command

```
usage: create_libs [-h] [-c] [-l LIB_NAME] langs [langs ...]

positional arguments:
  langs                 Give the name of languages for tree-sitter (php,
                        python, go ...)

optional arguments:
  -h, --help            show this help message and exit
  -c, --copy-to-workspace
                        Shall we copy the created libs to the present dir?
                        (default: False)
  -l LIB_NAME, --lib-name LIB_NAME
                        The name of the generated .so file
```


### Supported languages


### Tutorials

## A Quick Example

First run the above command to generate the libraries. 

In our settings we just use the `-c` flag to copy the generated `tree-sitter` library's `.so` file to our workspace.
And once copied, we place it under a directory called `tslibs` (It is in the .gitignore). But of course, if you are using linux then this command probably won't work and you will need to use our [tree-sitter-docker](https://github.com/autosoft-dev/tree-sitter-docker) image and manually copy the final .so file.

Another thing that we need before we can analyze any code file is an yaml with queries. We have suuplied one example query file
under [**queries**](https://raw.githubusercontent.com/autosoft-dev/tree-hugger/master/queries/example_queries.yml) directory. 

*Please note that, you can set up two environment variables `QUERY_FILE_PATH` and `TS_LIB_PATH` for the query file path and 
tree-sitter lib path and then the libary will use them automatically. Otherwise, as an alternative, you can pass it when creating any `*Parser` object*

Assuming that you have the necessary environment variable setup. The following line of code will create a `PythonParser` object

```python
from tree_hugger.core import PythonParser

pp = PythonParser()
```

And then you can pass in any Python file that you want to analyze, like so :

```python
pp.parse_file("tests/assets/file_with_different_functions.py")
Out[3]: True
```

`parse_file` returns `True` if success

And then you are free to use the methods exposed by that particular Parser object. As an example - 

```python
pp.get_all_function_names()
Out[4]:
['first_child',
 'second_child',
 'say_whee',
 'wrapper',
 'my_decorator',
 'parent']
```

OR

```python
pp.get_all_function_documentations()
Out[5]:
{'parent': '"""This is the parent function\n    \n    There are other lines in the doc string\n    This is the third line\n\n    And this is the fourth\n    """',
 'first_child': "'''\n        This is first child\n        '''",
 'second_child': '"""\n        This is second child\n        """',
 'my_decorator': '"""\n    Outer decorator function\n    """',
 'say_whee': '"""\n    Hellooooooooo\n\n    This is a function with decorators\n    """'}
```

### API reference
