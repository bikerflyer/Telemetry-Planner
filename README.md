# Telemetry Planner

Mission planning and survey design tool for marine ROV operations.

A self-contained, browser-based application for designing transects,
generating waypoint routes, managing survey boundaries, and exporting
mission plans. Built specifically for underwater Caulerpa survey and
eradication operations.

------------------------------------------------------------------------

## Core Capabilities

-   🗺 Interactive map-based mission planning (Leaflet)
-   📐 Grid and pattern route generation (lawnmower, spiral, custom
    legs)
-   📍 Manual waypoint placement and editing
-   📏 Distance, bearing, and coverage calculations
-   🧭 Transect definition and metrics
-   🐚 Sighting capture with structured ecological metadata
-   🧷 Overlay loading for previous surveys and comparison tracks
-   📦 Import viewer survey ZIP archives with separate overlays preserved
-   📦 Mission export (JSON / route plans / ZIP bundles)
-   🎥 Optional video + telemetry synchronisation support

------------------------------------------------------------------------

## Design Philosophy

-   Single-file, portable HTML application\
-   No frameworks, no build step\
-   Pure vanilla JavaScript\
-   Deterministic geospatial maths via Turf.js\
-   Field-ready and offline-capable

------------------------------------------------------------------------

## Intended Use

Designed for:

-   ROV-based environmental surveys\
-   Caulerpa monitoring and eradication missions\
-   Structured transect planning\
-   Boundary-based coverage operations\
-   Waypoint generation for autonomous or semi-autonomous vehicles

------------------------------------------------------------------------

## Architecture

-   Frontend: Vanilla JavaScript + Leaflet.js + Turf.js\
-   Geometry: Deterministic distance, bearing, and area calculations\
-   Export: Planner JSON, GPX, and mission bundles\
-   Archive round-trip: survey ZIP reload with overlay separation\
-   Optional backend integration for validation or processing

------------------------------------------------------------------------

## Roadmap

-   Advanced waypoint routing state machine\
-   Cross-track error correction\
-   Autopilot / MAVLink / Signal K export formats\
-   Coverage optimisation tools\
-   Multi-vehicle mission coordination

------------------------------------------------------------------------

## License

Copyright © 2026 Steve Harris\
Licensed under GNU GPL v3.0 or later


Check out this from Qgroundcontrol repo

Main files:

Survey grid generation: src/MissionManager/SurveyComplexItem.cc
Survey item definition: src/MissionManager/SurveyComplexItem.h
Shared transect mission logic: src/MissionManager/TransectStyleComplexItem.cc/.h

Core idea:

Convert survey polygon GPS points to local NED metres.
Build a bounding rectangle.
Generate parallel rotated lines at gridSpacing.
Clip/intersect those lines with the polygon.
Convert clipped line ends back to GPS.
Reorder into a lawnmower pattern.
Turn those into MAVLink mission waypoints.

The juicy bit is here: QGC converts polygon vertices with QGCGeo::convertGeoToNed, reads gridAngle and gridSpacing, then builds rotated transect lines.

Then it intersects those lines with the polygon, converts results back with convertNedToGeo, and adjusts entry/lawnmower ordering.

For your own tender/ROV planner, that file is the best reference. Search inside it for:

_rebuildTransectsPhase1WorkerSinglePolygon

That’s basically the grid generator.
