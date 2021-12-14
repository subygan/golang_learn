---
emoji: 🐭
title: Effective go practice
description: Effective go practices that I'm trying ot acquire over time
layout: base
type: page
---

## Formatting

There is no need to align content or comment `go fmt` does that for you.

```shell
go fmt <file_path>
```

formats the file that is given

## commentary

Go provides C-style /**/ block comments and c++ style // line comments.
`godoc` is used processes go source files to extract documentation about the contents of the package. 

Comments appear before top-level declaration, with no intervening newlines, are extracted along with the declaration to serve as explanatory text for the item.

Every package should have a package comment, a block of comment preceding the package clause. For multi-file packages, the package comment only needs to be present in one file, and any one will do. the package comment should introduce the package and provide information relevant to the package as a while. __It will appear first on the `go doc` page__

```go
/* 
packkage regexp implements a simple library for regular expressions.

The syntax of the regular expressions accepted is:

*/

package regexp
```