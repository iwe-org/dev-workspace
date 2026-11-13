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
iwe new --key data/plans/20260801-my-plan                        # create at an explicit key
iwe update -k data/features/foo --set stage=implemented         # set frontmatter
iwe rename <old-key> <new-key>                                   # move; references auto-update
iwe delete <key>                                                 # delete + reference cleanup
iwe normalize                                                    # run after manual edits
iwe schema validate                                              # the commit gate (exit 0 = clean)
iwe stats                                                        # counts, orphans, broken links
```

Filters are YAML (`$eq`, `$ne`, `$in`, `$gte`, `$exists`, …), not jq. Structural
anchors: `--includes`, `--included-by`, `--references`, `--referenced-by`,
`--roots`.

## Rules

- Move and delete with `iwe rename` and `iwe delete`, never `mv` or `rm`: they
  update every link in the graph. Check `git diff` afterwards; the reference
  updates are part of the change.
- Run `iwe normalize` after any manual edit, then `iwe schema validate` before a
  commit.
- `iwe schema validate --explain` shows which schema a document is bound to
  when a validation result is unexpected.
