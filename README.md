# GreenCorridor

A GPS-based ambulance traffic-signal priority system — prototype simulator.

GreenCorridor models how a fleet-tracked ambulance can request green lights along
its route in advance, using a safety-first state machine rather than an instant
red-to-green flip. This repo is a single self-contained HTML/JS simulation
(no build step, no dependencies) so it can be opened directly in a browser.

## Live demo
Open `index.html` in any browser, or visit the deployed link https://green-corridor-93abad.netlify.app/ 

## What it simulates

- **Round-trip run**: an ambulance travels Hospital Gate → City General → back,
  triggering junctions along the way in both directions.
- **Signal state machine** per junction:
  `NORMAL → PRE_EMPTION_REQUESTED → ALL_RED_CLEARANCE → AMBULANCE_GREEN → REVERTING → NORMAL`
  The all-red clearance step exists so cross-traffic already in the intersection
  can clear before the ambulance's direction gets the green — it's a safety
  step, not a delay.
- **Ambient cross-traffic**: small vehicles on the cross streets visibly queue
  during all-red/ambulance-green and resume once the junction reverts.
- **Green wave**: when a junction turns green, the next junction on the route
  is pre-primed so its own sequence starts faster when the ambulance arrives.
- **Optional reroute**: a Google-Maps-style toast offers a faster route via a
  Ring Road bypass mid-trip. Switching is always the user's choice, never
  automatic.
- **Fault injection + watchdog**: the "Inject fault" button simulates a sensor
  error (a junction forced green with no real ambulance nearby). A watchdog
  check runs every frame, cross-validates every active junction against real
  ambulance positions, and auto-reverts anything it can't verify — this is
  the mechanism that prevents a bad trigger from causing real intersection
  chaos.
- **Manual override**: an operator-style override button per junction, for a
  human to cancel a trigger.
- **GPS noise toggle**: shows a jittering "raw GPS" ghost next to the
  corrected position, illustrating why map-matching matters.
- **Audit log**: every state transition is timestamped and replayable.

## Stack
Vanilla HTML/CSS/JS, inline SVG for the map — no frameworks, no build tools.
Chosen deliberately for the prototype stage so anyone can clone and open it
with no setup.

## Status
Prototype / college project simulation. Not connected to real traffic
infrastructure. See the in-app legend and log for how the algorithm behaves.

## Next steps (not yet built)
- Siren/audio detection as a secondary confirmation layer
- Real hardware integration (ESP32 + physical signal demo)
- Backend + real GPS ingestion from a driver-facing mobile app
