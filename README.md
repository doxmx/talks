# talks

This repo contains presentations/talks/slides shared to DOxMX

# Using reveal.js for presentations

---

## Dependencies
- [pandoc](https://pandoc.org/installing.html)
- [reveal.js](https://github.com/hakimel/reveal.js/#installation)

---

## Generating presentations

- Generate a markdown file
- Transform the markdown to a presentation with `pandoc`

```Shell
$( type -p podman docker | head -1 ) run \
    --rm \
    --volume ${PWD}:/data:z \
    docker.io/pandoc/core:latest \
      -t revealjs \
      -s \
      -o my_presentation.html \
      my_presentation.md \
      -V revealjs-url=https://unpkg.com/reveal.js/ \
      -V theme=serif \
      --slide-level 3
```

---

## reveal.js themes

To use other themes, add the flag:

```Shell
-V theme=<THEME_NAME> \
```

The list of included themes is found in the [revealjs project](https://github.com/hakimel/reveal.js/tree/master/css/theme/source)

---

Now you can use your generated html file in a browser!

---
