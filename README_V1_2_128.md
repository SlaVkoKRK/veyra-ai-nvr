# VEYRA 1.2.128

## Light Threat: semantic, restrained Emitter Map

- Emitter Map no longer paints every raw bright pixel solid red.
- Debug reuses only the selected motion-supported reduced component from the existing Light Threat pass.
- Red now means **qualified optical emitter**.
- Unconfirmed candidates are shown with a soft amber overlay.
- Candidates vetoed by an animal track or reflection/surface guard remain visible diagnostically but are neutral/grey, never red.
- Runtime emitter qualification no longer accepts synthetic Gaussian halo by itself. Halo evidence also needs local illumination impact, a known person/vehicle carrier, or coherent temporal brightness growth.
- Debug overlay is alpha-blended and upscaled with linear interpolation, removing the heavy block/line appearance caused by nearest-neighbour expansion of the ~384 px analysis plane.
- No second Coral inference, no second connected-components pass, and no full-resolution Light Threat analysis were added.
