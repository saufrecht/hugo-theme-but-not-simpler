# hugo-theme-but-not-simpler
Hugo Theme with Solarized theme, automatic dark support. HTML, CSS, and Go only—no Javascript, SCSS, no social.

Version negative 1: code dump after transfer from local layout to theme layout.  Presumably not working since adjustments to have it work as a theme haven't been made.  Also, no example site or other necessities for sharing theme.

TODO:
- Get it to work as a theme, following standard hugo theme rules
  - with example site
  - with custom code and data
  - screenshots
  - setup and customization instructions in README
- remove tech debt
  - clean up and simplify the css
    - find and remove unused
    - find and remove duplicate
    - fine-tune the fonts and spacing
      - move all block spacing to margin-bottom where possible, eliminating margin-top
    - find a free font to use as default, rather than Quadraat
  - fix any TODOs in the code
  - make the navigation (forward and backward links) not ugly
  - get a cleaner way to split markdown-generated footnote material to its separate CSS grid area.
- new features
  - icon images for categories
  - integrate Docbook-generated chapters


## Page Layout

But Not Simpler is laid out in a three-column model.  The head column, which is on the left side in LTR languages, holds navigation content: tables of contents for pages and sections and 'Previous' links.  The middle column holds the primary content. The foot column, on the right side in LTR, holds foot/side notes and 'Next' navigation links.

The three-column model only fully displays on appropriately wide displays.  On smaller screens, the side columns merge as necessary.  This happens via the CSS Grid layout engine, without media queries.  If the layout comprised only three cells, one per column, then the cells would stack on top of each other, scrambling the content.  For example, this would put the page title below the page navigation, and push the body footnotes below the page footer.  So, this layout uses five rows, for a total of 15 cells:

|          |            |          |  Headnotes    |    main        | Footnotes   |
|----------|------------|----------|---------------|----------------|-------------|
| body     | header     |          |     1          |     2           |     3       |
| body     | main       | header   |     4          |     5           |     6        |
| body     | main       | main     |     7          |     8           |     9        |
| body     | main       | footer   |     10          |    11          |     12        |
| body     | footer     |          |     13         |     14           |     15        |


In a fully contracted layout, the cells render as
|cell|
|----|
|  1 |
|  2 |
|  3  |
|  4  |
|  5  |
|  6  |
|  7  |
|  8  |
|  9  |
| 10 |
| 11 |
| 12 |
| 13 |
| 14 |
| 15 |

Thus the body header footnotes are displayed directly beneath the body header main content, and so forth.

### Implementation in HTML and CSS

The body element is vertical flexbox.  Each cell in the flexbox is then filled with a single-row grid, using a shared column definition.  This keeps the layout consistent from row to row, and ensures all content from one row is displayed before any content from the next row regardless of viewport width.  Thus, all large-scale page layout happens in the body flexbox and its immediate children; no element moves out of its original grid cell.

The three logical columns of the layout use a CSS grid width of five grid columns, each 200px wide.  The main column fills three grid columns and the head and foot one column.  This resizes smoothly as the width shrinks.

