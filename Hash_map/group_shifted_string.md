

**Pattern:** HashMap + Canonical Signature

### Idea

Two strings belong to the same group if their characters have the **same shifting pattern**.

Example:

```
abc → +1, +1
bcd → +1, +1
xyz → +1, +1
```

So:

```
["abc", "bcd", "xyz"]
```

have the same signature.

### How to create the signature

Compare every character with the first character.

For:

```
abc
```

```
b - a = 1
c - a = 2
```

Signature:

```
(1, 2)
```

For:

```
bcd
```

```
c - b = 1
d - b = 2
```

Signature:

```
(1, 2)
```

Same signature → same group.

### Important: wraparound

Alphabet is circular:

```
z → a
```

So use:

```
(ord(c) - ord(first)) % 26
```

### Code

```
from collections import defaultdict

class Solution:
    def groupStrings(self, strings: list[str]) -> list[list[str]]:

        groups = defaultdict(list)

        for s in strings:
            first = ord(s[0])
            key = tuple((ord(c) - first) % 26 for c in s)

            groups[key].append(s)

        return list(groups.values())
```

### ⭐ Key insight

> **Create a canonical signature for each object. Objects with the same signature belong to the same group.**

This is the broader idea behind **Group Anagrams** too:

```
Group Anagrams
→ frequency signature

Group Shifted Strings
→ relative-position signature
```

### Complexity

If there are `N` strings and maximum length is `K`:

```
Time  → O(N × K)
Space → O(N × K)
```

**One-line takeaway:**

> **When a problem asks you to group "equivalent" things, think: Can I create a unique/canonical signature and use it as a HashMap key?**