# talks

This repo contains presentations/talks/slides shared to DOxMX

# Using reveal.js for presentations

---

## Dependencies
- [pandoc](https://pandoc.org/installing.html)
- [reveal.js](https://github.com/hakimel/reveal.js/#installation)

---

### One liners for installation

- pandoc: Use the appropriate installer for your distribution, example:
  ```bash
  sudo $( type -p dnf yum ) install pandoc -y
  ```

- reveal.js:
  ```
  revealjs_url=https://github.com/hakimel/reveal.js
  latest_release=$( curl -sI ${revealjs_url}/releases/latest |
                      awk -F/ '/^Location:/ {print $8}' |
                      tr -d '\r' )
  curl -sLO "${revealjs_url}/archive/${latest_release}.tar.gz"
  tar xzf ${latest_release}.tar.gz
  mv reveal.js{-${latest_release},}
  ```

---

## Generating presentation

- Generate a markdown file, then transform the markdown with `pandoc`:
  ```
  pandoc \
    -t revealjs \
    -s \
    -o my_notes.html \
    my_notes.md
  ```

---

## reveal.js themes

reveal.js comes with some themes included, find them in `reveal.js/css/theme/`.

To use other themes, simply:

```
pandoc \
  -t revealjs \
  -s \
  -V theme=<THEME_NAME> \
  -o my_notes.html \
  my_notes.md
```

Some examples are:
- night
- moon
- white

---

Now you can use your generated html file in a browser!

---
