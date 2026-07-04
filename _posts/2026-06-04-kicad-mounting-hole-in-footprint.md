---
title:  "How to add a mounting hole to a footprint in KiCad"
layout: post
categories: media
---

I ran into this problem recently and there isn't a clean way to do it.

The best solution in my experience is to define a pad using the "Add Pad" button.
<img width="90" height="60" alt="Screenshot 2026-07-04 at 4 45 16 PM" src="https://github.com/user-attachments/assets/ac0d2e04-4f87-4f89-a381-8f2fa02092af" />

Then set:
- Pad type: "NPTH, Mechanical" (Non-Plated Through-Hole)
- Pad shape: Circular
- (Pad) Diameter: your target hole diameter
- (Hole) Diameter: your target hole diameter


<img width="978" height="586" alt="Screenshot 2026-07-04 at 4 49 35 PM" src="https://github.com/user-attachments/assets/20b0c93f-4ed0-4e32-957b-b5fc053010a3" />
It's kind of hacky to have to define a pad where you don't actually want one, but this is just how it is.
For reference, the standard library does this too: `MountingHole:MountingHole_2.1mm`


<img width="977" height="778" alt="Screenshot 2026-07-04 at 4 44 55 PM" src="https://github.com/user-attachments/assets/30801d4b-f940-4626-9cc8-376773be093c" />
