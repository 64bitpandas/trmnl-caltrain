# Caltrain TRMNL Plugin

![example screenshot](image.png)

Real-time Caltrain departure times for your TRMNL e-ink display. Shows next departures for both northbound and southbound trains from your selected station.

## Quick Start

1. Visit the TRMNL plugin page (or create a Private Plugin)
2. Select your Caltrain station from the dropdown
3. Choose your preferred layout
4. Set polling interval (recommended: 5-15 minutes)

## Layouts

| Layout          | File                     | Description                                    |
| --------------- | ------------------------ | ---------------------------------------------- |
| Full Screen     | `full.liquid`            | Split view with 3 trains per direction         |
| Half Vertical   | `half_vertical.liquid`   | Compact stacked view, 2 trains per direction   |
| Half Horizontal | `half_horizontal.liquid` | Wide compact view, 2 trains per direction      |
| Quadrant        | `quadrant.liquid`        | Minimal view showing next train each direction |

## Train Types

- **LOCAL** — Stops at all stations (train numbers starting with 1 or 2)
- **LIMITED** — Skips some stations (train numbers starting with 3-6)
- **EXPRESS** — Fastest service, limited stops (train numbers starting with 7+)

## Data Source

This plugin uses Caltrain's public GTFS real-time feed at `caltrain.com/gtfs/stops/{station}/predictions`.

## License

[MIT](LICENSE). Data provided by Caltrain.
