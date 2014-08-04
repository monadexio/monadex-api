Introduction
============

AGLIO
-----
Install aglio via NPM. You need Node.js installed and you may need to use `sudo` to install globally:

```bash
npm install -g aglio
```

```bash
# Default template
aglio -i input.md -o output.html

# Get a list of built-in templates
aglio -l

# Built-in template
aglio -t slate -i input.md -o output.html

# Custom template
aglio -t /path/to/template.jade -i input.md -o output.html

# Run a preview server on http://localhost:3000/
aglio -i input.md -s

# Print output to terminal (useful for piping)
algio -i input.md -o -

# Disable condensing navigation links
aglio -i input.md --no-condense -o output.html
```
