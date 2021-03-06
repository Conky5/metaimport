## metaimport

`go get github.com/nishanths/metaimport`

Specify a Git repository and `metaimport` will generate a directory of
HTML files containing `<meta name="go-import">` tags for the Go packages
in the repository, suited for your vanity URL.

These tags are used by commands such as `go get` to determine how to fetch 
source code. See `go help importpath` for details.

The program can also optionally create `<meta name="go-source">` tags, as used by 
[godoc.org](https://github.com/golang/gddo/wiki/Source-Code-Links).

## Example

```
$ metaimport -o html example.org/myrepo https://github.com/user/myrepo
```

will generate HTML files like:

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="go-import" content="example.org/myrepo git https://github.com/user/myrepo">
    <meta http-equiv="refresh" content="0; url='https://godoc.org/example.org/myrepo'">
  </head>
  <body>
    Redirecting to <a href="https://godoc.org/example.org/myrepo">https://godoc.org/example.org/myrepo</a>
  </body>
</html>
```

Once the HTML files are generated, you can serve them at the root of your domain 
(`example.org` in this example) with something like:

```
$ cd html/example.org
$ file-server . # serve files in the directory over https
$ go get example.org/myrepo # should now work
```

## Usage

See `metaimport -h`.

```
usage: metaimport [-branch branch] [-godoc] [-o dir] [-redirect] <import-prefix> <repo>

metaimport generates HTML files with <meta name="go-import"> tags as expected
by go get. 'repo' specifies the Git repository containing Go source code to
generate meta tags for. 'import-prefix' is the import path corresponding to
the repository root.

Flags
   -branch    Branch to use (default: remote's default branch).
   -godoc     Include <meta name="go-source"> tag as expected by godoc.org (default: false).
              Only partial support for repositories not hosted on github.com.
   -o         Output directory for generated HTML files (default: html).
              The directory is created with 0755 permissions if it doesn't exist.
   -redirect  Redirect to godoc.org documentation when visited in a browser (default: true).

Examples
   metaimport example.org/myrepo https://github.com/user/myrepo
   metaimport example.org/exproj http://code.org/r/p/exproj
```
