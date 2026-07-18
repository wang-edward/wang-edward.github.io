---
title:  "How to copy paste PCB layouts in KiCad"
layout: post
categories: media
---

You can bring circuits into KiCad from other projects by copy pasting.
But when you are "importing" from more than one project, the references (e.g. C12) will collide (e.g. both projects will define C12 but there can only be one).
This can be resolved using the "Annotate Schematic" tool.
But changing the references breaks the link between the schematic and PCB layouts. So that means you can't paste the layouts.

The solution is to use hierarchal sheets and reannotate both your project and the examples with "Numbering: First free after sheet number X 100".
You can use "Edit Page Number" in the Hierarchy panel to make the page numbers line up.
If you're working with a clone of the example project then this is free.
You also need to make the hierarchal schematics identical wrt symbol positions so that the references line up.

Now you can copy paste the example layout in.
Next, do "Update PCB from Schematic".
It's important to have "Re-link footprints to schematic symbols based on their reference designators" checked.
This is because KiCad uses a map of UUIDs to keep track of which symbols are linked to which footprints.
When you copy paste from another file, the UUIDs are gonna be messed up so you need to relink them.

Now ratsnest should work properly.
