# Description

While trying to integrate ratatui with TIC80 phantasy retro console, though
successful, it became apparent that the memory requirements are too much for
the platform.

The main reason is the size of the buffer(s).
The main culprit is the `Cell` type, which has many fields and is not very optimized.

The approach will be to try to cut back and pack data as much as possible, while
still allowing functional, useful TUI experiences.

## Proposed cuts

- [ ] `Color` with 16 colors (plus reset, if really needed?)
        _No RGB, no 8bit indexed_
- [ ] Limit to ANSI ASCII chars
        _A single byte_
- [ ] Limit available modifiers
        _To be decided_
- [ ] No underline color specifier.
        _This is already optional._

## Implementation

### 'ascii' feature

From all the proposed changes, this is the easiest and the most significant,
because the current type `CompactString` takes a minimum of 24 bytes, which
represents more than half the total cell size.

We will make the `symbol` field a `[u8]` (a single byte array).

> We choose to wrap it in an array, because it has no cost in terms of space,
> and some functionality already assumes that `symbol` is a collection.
