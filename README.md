# Fix CodeQL python setup script

Using CodeQL with the GitHub runner for Python application is work. However when it is used with a self-hosted runner it fails. Investigating into it shows
that GitHub runner `ubuntu-latest` has python2 installed where as self-hosted runner does not have `python2` installed. However the `if` condition not suppose to fail.

https://github.com/github/codeql-action/blob/main/python-setup/install_tools.sh#L31
```sh
if command -v python2 &> /dev/null; then
```

The behaviour of the `command` is different shell to shell. Best solution to apply in this situation is to redirect stdout and stderr separately as below.
```sh
if command -v python2 >/dev/null 2>&1; then
```
