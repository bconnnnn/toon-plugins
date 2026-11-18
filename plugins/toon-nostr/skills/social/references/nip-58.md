# NIP-58: badges

A badge is defined by an issuer, awarded to keys, and shown by the keys that wear it.

## Kinds

- `30009` badge definition, addressable. Tags: `["d","<badge id>"]`, `["name","..."]`,
  `["description","..."]`, `["image","<url>","<width>x<height>"]`, optional `["thumb","<url>"]`.
- `8` badge award, by the issuer, immutable. Tags: `["a","30009:<issuer>:<badge id>"]` and one
  `["p","<awardee>"]` per awardee. `content` is empty.
- `10008` profile badges, replaceable, one list per key. Pairs of tags, in order:
  `["a","30009:<issuer>:<badge id>"]` then `["e","<award id>"]`.
- `30008` badge set, addressable by its `d` tag. A set groups the same definition and award
  pairs. Include a set in the profile list with `["a","30008:<wearer>:<set id>"]`.

## Publish

Define, then award (the issuer):

- **kind** `30009`
- **content** empty
- **tags** `[["d","early-reader"],["name","Early Reader"],["description","Read the relay in its first week"],["image","https://example.org/early.png","256x256"]]`

and

- **kind** `8`
- **content** empty
- **tags** `[["a","30009:<your key>:early-reader"],["p","<awardee key>"]]`

Wear a badge you were awarded (the awardee):

- **kind** `10008`
- **content** empty
- **tags** `[["a","30009:<issuer>:early-reader"],["e","<award id>"]]`

## Read

```json
{"kinds":[8],"#p":["<key>"]}
```

Read the badges that key displays:

```json
{"kinds":[10008],"authors":["<key>"]}
```

A badge counts only if the award's `a` tag names a definition by the same issuer, and the `p` tag
names the wearer.

## Notes

- Replacing kind `10008` drops the badges and sets you leave out: send the whole list.
- Do not award a badge for payment you cannot show; an award is public and cannot be silently taken back
  except by a deletion request (`nip-09.md`).

[NIP-58 defines the event kinds and tag order.](https://github.com/nostr-protocol/nips/blob/999f9bfbf5fe00d5c2711fd24badb4e56748c9bc/58.md)
