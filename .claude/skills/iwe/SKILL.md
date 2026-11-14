---
name: iwe
description: The iwe CLI reference for this workspace — reading documents and hubs, finding by title, text, frontmatter or graph position, creating, renaming and deleting without breaking links, normalizing and validating. Use before running an iwe query or restructuring documents, or when an iwe command fails.
---

# The iwe CLI

Every command runs from the workspace root. A document's key is its
extension-less path (`data/spec/timer`).

``` bash
iwe retrieve -k data/spec/timer                                  # read a doc
iwe retrieve -k data/plans --expand-includes 1                   # hub + children
iwe find --fuzzy timer -f keys                                   # fuzzy title+key match
iwe find --lexical "session log storage" -f keys                 # full-text ranking
iwe find --included-by data/plans -f keys                        # all docs under a hub
iwe find --references data/spec/timer -f keys                    # backlinks
iwe find --filter '{stage: done}' --included-by data/plans-archive:0 -f keys  # frontmatter query
iwe tree -k data/plans -d 2                                      # subtree overview
iwe new "My plan" --key data/plans/20260801-my-plan --content "" # create at an explicit key; the title is required
iwe update -k data/features/foo --set stage=accepted --unset status  # set and remove frontmatter fields
iwe rename <old-key> <new-key>                                   # move; references auto-update
iwe delete <key>                                                 # delete + reference cleanup
iwe normalize                                                    # run after manual edits
iwe schema validate                                              # the gate (exit 0 = clean)
iwe stats                                                        # counts; Broken Links and Orphans sections only when there are any
```

Filters are YAML (`$eq`, `$ne`, `$in`, `$gte`, `$exists`, …), not jq. Structural
anchors: `--includes`, `--included-by`, `--references`, `--referenced-by`,
`--roots`.

## Rules

- Move and delete with `iwe rename` and `iwe delete`, never `mv` or `rm`: they
  update every link in the graph and print how many documents they changed. The
  reference updates are part of the change; in a git repository, `git diff`
  shows them. `iwe delete` removes inclusion links to the document and turns
  inline links to it into plain text, which is then yours to reword or remove.
- `iwe new` reads the document body from piped input when it gets no
  `--content`, and waits for it; pass `--content ""` for an empty document.
- `--set FIELD=VALUE` parses the value as YAML, so a value that contains a colon
  and a space needs quotes of its own:
  `--set 'description="Version 0.2.0: break timers."'`.
- Run `iwe normalize` after any manual edit, then `iwe schema validate` before
  you call the change done.
- `iwe schema validate --explain` shows which schema a document is bound to
  when a validation result is unexpected.
