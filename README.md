## Xeno:Lab Blog

0. Install `pelican` and `ghp-import`:
```
   $ pip install pelican ghp-import
```

1. Add new content to the `content` branch, **never** `master`.

```
   $ git checkout content
```
   
2. Add new files to content/YYYY-MM-DD-slug.md

```
   $ vi content/2018-10-03-awesome.md
```

3. Publish to `master` branch using the Makefile

```
   $ make publish
```

4. Commit new content to the ```content``` branch

```
   $ git add .
   $ git commit -m 'sweet new article about ...'
   $ git push origin content
```