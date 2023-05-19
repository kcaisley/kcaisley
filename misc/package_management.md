# DNF and RPM package management:

Check current version of an installed RPM
`rpm -q <packagename>`

Check current versions of remote and installed versions of a package
`dnf info <packagename>`

List contents of installed RPM
`rpm -ql <packagename>`

List contents of remote repository package:
`dnf repoquery -l spdlog`


