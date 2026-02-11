# Showboat Command Reference

Run `showboat --help` to get the full, up-to-date list of commands and options.

For help on a specific command, run `showboat <command> --help`.

## Tips

Pipe code from stdin when the command is complex or multi-line:
```bash
cat script.sh | showboat exec demo.md bash
echo "This is a note" | showboat note demo.md
```
