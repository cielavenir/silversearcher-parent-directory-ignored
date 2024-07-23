silversearcher-parent-directory-ignored

## Summary

Under tmp directory, `ag Hello ../src` does not work.

```
$ (cd tmp && ag Hello ../src)

$ (cd tmp && rg Hello ../src)
../src/hello.txt
1:Hello World

$ (cd tmp && pt Hello ../src)
../src/hello.txt
1:Hello World
```

Do not get confused with -u (--unrestricted), it is different.

```
$ (cd tmp && ag -u Hello ../src)
../src/src/hello.txt
1:Hello Recursive World
../src/hello.txt
1:Hello World

(cd tmp && rg -u Hello ../src)
../src/hello.txt
1:Hello World
../src/src/hello.txt
1:Hello Recursive World

$ (cd tmp && pt -U Hello ../src)
../src/src/hello.txt
1:Hello Recursive World
../src/hello.txt
1:Hello World
```

## Analysis

The thing is the existence of ../src/.gitignore specifying recursive "src".

When `(cd tmp && ag --debug Hello ../src)` is run, this is shown:

```
DEBUG: Loading ignore file ../src/.gitignore.
DEBUG: added ignore pattern src to 
DEBUG: search_dir: path is '../src', base_path is '/foo/src/', path_start is '../src'
DEBUG: file src ignored because name matches static pattern src
```

`../src/.gitignore` should ignore only directories under ../src/, but ../src itself is ignored.
