---
theme: post
published: true
title: General tips and tricks
---
# 17 September

### Regexp refactoring

Use VSCode regexp replace with values:
```
_(.*)Details\(
_$1DetailsBuilder(
```

to change

`class _AccountCloseDetails(_OrderDetailsBuilder):`

into

`class _AccountCloseDetailsBuilder(_OrderDetailsBuilder):`

---
