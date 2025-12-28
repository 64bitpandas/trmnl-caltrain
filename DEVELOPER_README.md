# Developer Guide: Caltrain TRMNL Plugin

Instructions for publishing this plugin to the TRMNL marketplace.

## Prerequisites

- A TRMNL account at [dashboard.trmnl.com](https://dashboard.trmnl.com)
- Access to create Private Plugins or submit to the marketplace

## Setup Steps

### 1. Create a New Private Plugin

1. Log into your TRMNL dashboard
2. Navigate to **Plugins** → **Private Plugins**
3. Click **Create New Plugin**
4. Enter a name (e.g., "Caltrain Departures")

### 2. Configure Polling Settings

In the plugin settings, configure the following:

| Setting         | Value                                                                    |
| --------------- | ------------------------------------------------------------------------ |
| Strategy        | `polling`                                                                |
| Polling Verb    | `GET`                                                                    |
| Polling URL     | `https://www.caltrain.com/gtfs/stops/{{ caltrain_station }}/predictions` |
| Polling Headers | (leave empty)                                                            |
| Polling Body    | (leave empty)                                                            |

The `{{ caltrain_station }}` variable will be replaced with the user's station selection.

### 3. Add Form Fields

Copy the contents of `settings.yml` into the **Form Fields** section. This defines:

- **caltrain_station**: Dropdown with all 30 Caltrain stations
- **author_bio**: Plugin description and contact info

### 4. Add Templates

For each layout size, paste the corresponding template:

| TRMNL Layout           | Template File            |
| ---------------------- | ------------------------ |
| Full Screen            | `full.liquid`            |
| Half Vertical (Tall)   | `half_vertical.liquid`   |
| Half Horizontal (Wide) | `half_horizontal.liquid` |
| Quadrant (Small)       | `quadrant.liquid`        |

### 5. Configure Polling Interval

Recommended polling interval: **15 minutes**

## API Details

### Endpoint

```
GET https://www.caltrain.com/gtfs/stops/{station_urlname}/predictions
```

### Response Structure

```json
{
  "data": [
    {
      "stop": {
        "title": "Station Name",
        "field_location": [{"latlon": "lat,lon"}]
      },
      "predictions": [
        {
          "TripUpdate": {
            "Trip": {
              "TripId": "123",
              "RouteId": "route_id"
            },
            "StopTimeUpdate": [
              {
                "StopId": "70011",
                "Arrival": {"Time": 1234567890, "Delay": 0},
                "Departure": {"Time": 1234567890}
              }
            ]
          }
        }
      ]
    }
  ],
  "meta": {
    "routes": {...}
  }
}
```

### Direction Detection

- **Odd StopId** → Northbound (toward San Francisco)
- **Even StopId** → Southbound (toward Gilroy)

### Train Type Detection

Based on train number prefix:

- `1xx`, `2xx` → Local
- `3xx`, `4xx`, `5xx`, `6xx` → Limited
- `7xx`+ → Express/Bullet

## Publishing to Marketplace

1. Test your plugin thoroughly with a TRMNL device
2. Take screenshots of each layout for the listing
3. Submit via the TRMNL developer portal
4. Include:
   - Plugin name and description
   - Screenshots for each layout
   - Author contact information
   - GitHub repository link (optional)

## Troubleshooting

**No data displayed:**

- Caltrain API may be down during late night hours
- Check if the station has active trains

**Wrong direction shown:**

- Verify StopId modulo logic matches current GTFS feed
- San Francisco only shows southbound (terminal station)
- Gilroy only shows northbound (terminal station)

## Resources

- [Caltrain GTFS Feed](https://www.caltrain.com/schedules/realtime.html)
- [TRMNL Developer Docs](https://usetrmnl.com/docs)
- [Liquid Template Language](https://shopify.github.io/liquid/)
