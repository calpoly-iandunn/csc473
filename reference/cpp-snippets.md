---
layout: page
permalink: /glossary/cpp-snippets/
title: "C++ Snippets"
---

## Read entire file into string

Note: this only works for ASCII files, Unicode support not guaranteed

```code
std::ifstream FileHandle(FileName);
std::string String;

FileHandle.seekg(0, std::ios::end);
String.reserve((uint) FileHandle.tellg());
FileHandle.seekg(0, std::ios::beg);

String.assign((std::istreambuf_iterator<char>(FileHandle)), std::istreambuf_iterator<char>());
```

---
