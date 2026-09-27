# VEYRA 1.2.110

## Manual GLARE Masks + High Risk Zones

### Manual GLARE Masks â€” per camera
- New polygon type in Camera Settings â†’ Masks: **Manual GLARE mask**.
- Every camera owns its own list of GLARE polygons and overlap threshold.
- The editor can load the latest **Glare Recovery** snapshot from the same camera as the drawing background.
- A known static/doorway GLARE false positive inside the polygon is vetoed.
- Threat Guard can override the manual mask for a coherent real emitter/carrier, so the polygon is not a permanent blind spot for a person with a headlamp or an approaching vehicle.
- Runtime status exposes matched manual mask and overlap for diagnostics.

### High Risk Zones â€” per camera
- New polygon type in Camera Settings â†’ Masks: **High Risk Zone**.
- Every zone has its own name, object classes and repeat interval.
- Zone membership uses the bottom-centre point of the object bbox, which better represents a person/vehicle entering a doorway, gate or parking place.
- Entering a risk zone generates an additional immediate wake-up once the event is confirmed.
- While the object remains inside the zone, repeats use the zone cadence instead of the normal active/stationary cadence.
- High-risk repeats are also delivered to Internet notification providers that accept normal confirmed alerts even when generic repeats are disabled.
- Notification payload includes `risk_zone`, `risk_zones` and `high_risk`.

### Compatibility
- Existing Motion/Object masks remain unchanged.
- Existing Dynamic Glare Mask / Recovery remains automatic and independent.
- No additional Coral inference, full-frame resize, optical flow or second detector path was added.
