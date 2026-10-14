---
name: gridfinity-layout-planning
description: Plan a Gridfinity layout: tile the baseplate for the user's printer (default Prusa CORE One), track printed tiles, then validate bin footprints (optional half units, heights in 7 mm units).
---

# Gridfinity layout planning

Use this when the user wants to cover a surface (shelf, drawer, niche) with Gridfinity. Work through the steps in order and keep replies short: the diagram carries the information, so do not repeat it in text.

## Conventions (keep them identical across every diagram)
- One cell = 42 mm. Show "1 cell = 42 mm" in each diagram.
- Columns are numbered left to right, rows back to front (row 1 is against the back wall or mirror). A cell is written (column, row).
- Tiles are named T1, T2, ... in the format TN. Label each tile with its name and size, e.g. "T4 3×5" (rows × columns, depth first).
- Colours: unprinted tiles grey, printed tiles green. Obstructions (holders, pipes) are shown as a distinct blocked area with no cells.
- Label the back edge (e.g. "Mirror / back wall") and the front edge. Draw diagrams inline as SVG.

## Printer (ask once, default available)
Ask for the printer make and model in the same message as the area questions in step 1, and state the default so the user can simply skip it, e.g. "Printer? (default: Prusa CORE One, 250 × 220 mm bed)". If the user does not name a printer, or says "default" or "same", use the Prusa CORE One. Do not ask again once it is known in the conversation.
- Prusa CORE One (default): 250 × 220 mm bed, so the largest tile is 5×5 cells (210 mm). The CORE One+ has the same bed.
- Any other printer: find its bed size (look it up if the model is well known, otherwise ask the user for the X × Y print area). The largest tile is floor(bed X / 42) by floor(bed Y / 42) cells; allow for any margin the chosen baseplate model needs, and tell the user the resulting maximum tile size.

## Half-unit bins (optional, off by default)
Half-grid bins (21 mm steps) are a common practice but only work if the chosen baseplate and bin set support them (for example, Gridfinity-SP lists half-grid bin support with a minimum size). Do not raise them unless the user asks or uses a .5 coordinate; at step 5, add one line saying half-unit bins are available by using .5 values. When enabled:
- Coordinates may use .5. A bin's top-left coordinate t starts the bin at distance t - 1 cells from the back/left edge of the plate; its bottom-right coordinate b ends it at distance b cells. Size on each axis = b - t + 1 cells, in steps of 0.5.
- So whole numbers behave exactly as before. Top-left 1.5 starts in the middle of cell 1; bottom-right 3.5 ends in the middle of cell 4. Example: (1,1) to (2.5,1.5) is 2.5 × 1.5 cells (105 × 63 mm).
- Check each bin against the baseplate's half-grid support before accepting it, and say so if the chosen baseplate does not offer it.
- Overlap checks work on half-cell resolution (each cell split 2 × 2). A bin may use half of a usable cell, but never any part of a blocked cell.
- Draw faint half-cell lines inside the cell grid, keep the thick borders between bins, and list each footprint in mm (cells × 42).

## Bin heights (optional)
Bin height is counted in Gridfinity height units: 1u = 7 mm (confirm against the bin generator or model the user is using). Heights are optional; if the user gives none, show footprints only and do not invent heights.
- Format: add the height after the coordinates, e.g. "(1,1) to (4,3), 6u".
- Convert to mm as u × 7. Bins with a stacking lip stand taller than that by the lip height (about 4.4 mm in the standard design), and the baseplate adds its own thickness (about 5 mm, depending on the model). State which of these you included.
- If the user gave headroom (a shelf, a mirror ledge, a cabinet above) in step 1, check bin height plus baseplate thickness against it and flag any bin that does not fit.
- If the user gives the height of the items to store, suggest a height in units (item height divided by 7, rounded) and say whether the bin should be shorter or taller than the item; tall narrow bottles are often better in shorter bins so they are easy to grab.
- In the cell grid, label each bin with its height (for example "6u") in one of its cells and list heights in mm next to the footprints.

## Steps
1. **Get the area and printer.** The user gives the dimensions. Ask which side is horizontal, whether anything obstructs part of the area (note its width and depth), whether the surface is flat with or without a rim, any headroom limit above the plate, and the printer (see above, with its default). Compute whole cells per axis (floor of mm / 42) and report the slack in mm. If an obstruction takes part of the area, work out the usable cells and any L shape.
2. **List the tiles to print.** Split the grid into baseplate tiles no larger than the printer's maximum tile size. Give a small table of quantity and size. Before finalising, confirm the chosen baseplate model actually offers each tile size (check its file list).
3. **Draw the tile layout.** Numbered tiles T1..TN, all grey (unprinted), with obstruction and edges labelled and the cell grid faintly visible. Stagger seams so no seam lines up across rows, including at steps in an L-shaped plate; if one does line up, rearrange the same tiles instead of adding new ones.
4. **Update on progress.** When the user reports printed tiles, redraw the layout with those tiles green. Say how many are done and which remain; suggest a print order where each new tile clips onto one already printed.
5. **Final tile layout and cell grid.** When every tile is printed, redraw the tile layout one last time (all green). Then give the numbered cell grid: column and row numbers on the axes, unused cells grey, blocked cells marked, and a short note on the (column, row) convention and how many cells are usable. Mention in one line that half-unit sizes (.5) and bin heights in units (u) are optional extras. Give assembly steps (dry fit, glue dabs, cure time) only if the user wants them.
6. **Collect bins.** The user returns each bin as its top-left and bottom-right cell, inclusive, as (column, row), optionally with a height in units.
7. **Validate and draw.** Check that every bin is inside the usable area, avoids blocked cells and does not overlap another bin, and check heights against any headroom. Report any conflict before drawing. Then redraw the cell grid with used cells green and thick borders between bins, unused cells grey, and give each bin's footprint in mm (cells × 42) plus its height in mm when one was given.

## Practical notes
- Use PETG (not PLA) for plates and bins that live near water or heat.
- Prefer a baseplate whose bins click or latch in if the user dislikes bins sliding; confirm by a small test print before printing every tile.
- Bins may cross tile seams without any problem.
- Never claim measurements from a photo are exact; ask the user to measure.
