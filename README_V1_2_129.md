# VEYRA 1.2.129

## Light Threat: optical-only IR reflection rejection

- Restores the restrained v1.2.116/1.2.117 Emitter Map sensitivity profile: `min_y=178`, `scene_delta=72`, `core_contrast=24`, `halo_threshold=0.10`, `impact_min=5`, gate `0.12`.
- Light Threat no longer depends on dog/cat/animal tracking at all. Animal classes may be disabled and the glare alarm logic still works the same.
- IR-reflective fur/eyes/plates are rejected optically: brightness growth alone is no longer enough; an unclassified source needs real local light impact plus compact bloom/low surface score, or a known person/vehicle carrier.
- Peak Rescue remains available for tiny distant headlamps/headlights, but a rescued one-pixel candidate cannot be promoted by brightness growth alone; it needs real scene-light impact or a person/vehicle carrier.
- Debug returns to a thinner, darker v1.2.116/1.2.117-style core + halo presentation. Red is reserved for a qualified optical emitter; reflections remain neutral and unconfirmed candidates stay faint amber.
- IR Transition Guard remains unchanged: entering stable `NIGHT_IR` arms an ~8 s Light Threat suppression window, clears emitter history and pauses Adaptive Static Light learning while Motion/Coral continue normally.
- No second Coral inference, no optical flow, no second full-frame connected-components pass and no full-4K Emitter Map rebuild.
