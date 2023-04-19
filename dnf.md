to list all packages a certain package depends on:

```
dnf repoquery --requires --resolve <package>
```

to do the reverse, and search for all packages that depend on the specified:

```
dnf repoquery --alldeps --whatrequires
```

to search the same as the above, but only within locall installed packages:

```
dnf repoquery --installed --whatrequires
```

to search for what repos include a file, using a wildcard:

```
dnf provides 'filena*'
```

to refresh repositories for software updates, and then update all packages:

```
sudo dnf upgrade --refresh
```

