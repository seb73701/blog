---
title: Sortable tables and typed columns
description: 
published: true
date: 2023-04-21T11:06:10.105Z
tags: 
editor: markdown
dateCreated: 2023-04-21T11:06:04.438Z
---

# Sortable tables and typed columns
The general topic on table syntax is already huge.

I want to focus on some related special-purpose features here.

## Table structure
It often makes sense to offer readers the possibility to sort and perhaps filter tables by a different criterion than the source code order.

HTML, still the default output format, helps with its row grouping: a table consists of at most one header group `<thead/>` (contains column captions), at most one footer group `<tfoot/>` (contains column totals) and at least one body group `<tbody/>` – rows should only be sorted within a body group. With some variants of grid tables you can use strings of hyphens - to separate rows on the one hand and strings of equals signs = to separate row groups on the other hand, which neatly is the same hierarchical distinction as in underlined Setext headings.
The type of row group intuitively follows from the total number and position in the sequence: a single-group table only has a body group, otherwise the first group is always the header group and the second group is always a body group, and if the table has three row groups or more, the last one is always the footer group and any others are additional body groups. It follows that it should be possible to have empty header and footer groups in CM input to suppress them for output.

HTML also offers column grouping with its optional and orthogonal `<colgroup/>` and `<col/>` element types, but no further classification of columns. I believe it makes sense to implicitly derive this from the layout of the table header row group instead of introducing another horizontal table separator than the pipe `|` for explicit column grouping, although colon `:`, exclamation mark `!` and hash sign `#` perhaps could be used for this purpose; double pipes `||` may also work.

Header cells may be scoped explicitly in HTML with `scope` and `header` attributes, but I think this should at most happen implicitly in Commonmark.

So far, this describes which parts can be sorted, but we also need to know which data should be sorted by – and how.

- Are tables considered to be sortable by default?
- Can tables only be sorted by columns?

## Column data typing
### Manual column alignment
It’s common for Markdown (pipe) table extensions to support manual specification of the horizontal alignment of all cells in a column. This is most often done with the colon `:` as an optional marker within the table header separator line:

- `|---|` unspecified horizontal alignment, defaults to either start/left alignment or to horizontally centered content or to automatic alignment
- `|:--|` cell content shall be start/left aligned
- `|--:|` cell content shall be end/right aligned
- `|:-:|` cell content shall be either horizontally centered or be justified

### Automatic column alignment

However, the specified horizontal alignment for cells within a column usually depends on the cell content type. Numeric content, possibly including a unit symbol, should mostly be aligned to the right, short textual content should usually be start-aligned (which is on the left for European scripts), longer texts (especially block content like multiple paragraphs) might be justified preferably, some very short texts (i.e. single words or symbols) should be centered. Vertically, most content should be top-aligned, while column headers should be bottom-aligned. Short content that is centered horizontally may also be centered vertically.

I think it makes sense to specify the type of cell content explicitly and derive the preferred horizontal (and possibly vertical) cell alignment from that. This is more like spreadsheet applications handle that. It is also a more semantic and less stylistic approach.

- `|---|` default cell content
- `|.--|` numeric cell content, floating point: aligned at decimal separator, decimal grouping ignored for sorting, Unicode digits and vulgar fractions should be converted
- `|,--|` numeric cell content, fixed point: right aligned, decimal grouping ignored for sorting
- `|:--|` temporal cell content, clock times (usually also works for durations): 12h format with AM/PM shirts semantically
- `|;--|` list cell content: line break after each comma or semicolon
- `|!--|` row header cell content, label: bold, fixed
- `|?--|` boolean cell content, checkmarks and other symbols: centered
- `|/--|` calendrical cell content, dates: month and day names should sort semantically and not alphabetically, incomplete and approximate dates shall be handled, display date format may be localized or rather short and long notation stills selected automatically
- `|$--|` monetary cell content: automatic currency conversion may be applied, unit may come before amount
- `|%--|` relative cell content, percentages: Unicode vulgar fractions and ratios supported, percent and permille signs recognized, ppm notation recognized
- `|°--|` radial cell content, angles and bearings: normalization for sorting, compass directions recognized, π and τ recognized, arcus functions recognized
- `|#--|` unsigned integers, e.g. hexadecimal cell content or ordinal ranks
- `|+--|` numeric cell content, decimal integers, implied sign
- `|*--|` numeric cell content with unit symbol, engineering notation: sorting may involve conversion to canonical unit, respect SI prefixes
- `|~--|` name cell content, untranslatable: Unicode collation algorithm for sorting
- `|_--|` form cell content: should not be used for sorting key
- `|&--|` space-separated list cell content: line break after each word, sorting cell content together with cells is allowed
- `|@--|` URL cell content: automatic linking and truncation, sort by TLD first(?), sort IPv4 and v6 numerically, punycode and percent-decoding
- `|"--|` long textual cell content, prose or notes: justified, may be considered not an appropriate sort key
- `|'--|` short textual cell content, labels: start-aligned, appropriate as sort key, transliteration / romanization / sluggification may be applied before sorting
- `|^--|` numeric cell content, scientific notation: sorting may involve canonicalization, but no unit conversion
- `|=--|` row (sub-)total cell content; may be incompatible with row group syntax, then use !
- `|\--|` static cell content which stays the same when other columns are sorted, e.g. ordinal rank number

The number of repetitions of the marker character may indicate a minimum or fixed width, possibly before and after separators, e.g.:

- `|////-//-//|` ISO 8601 dates
- `|::-::|` hours and minutes or minutes and seconds
- `|$-$$|` financial amount with two decimal places
- `|-.|` variable-length decimal floating point number without leading zero
- `|###|` fixed-width number with three digits and leading zeros
- `|+-++++++|` fixed-width integer with six digits with invisible leading zeros after sign
- `|#-#-#-#|` IPv4 address (v6 is complicated due to shorthand notation, should be sorted within `@`)


## References
- https://html.spec.whatwg.org/multipage/tables.html 2

source = https://talk.commonmark.org/t/sortable-tables-and-typed-columns/4072