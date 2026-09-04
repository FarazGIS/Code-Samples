# Shortest Distance Between ZIP Boundaries

This browser sample uses Azure Maps to display two US ZIP-code boundaries and calculate a driving route between them. It reports both:

- **Boundary-to-boundary route:** the portion of the route between the point where it leaves the origin ZIP boundary and the point where it enters the destination ZIP boundary.
- **Full center-to-center route:** the complete driving route between the locations returned by Azure Maps for the two ZIP codes.

The map draws the origin and destination boundaries, the full route, the measured route segment, and the selected boundary-crossing points.

## Prerequisites

- An [Azure Maps account](https://learn.microsoft.com/azure/azure-maps/quick-demo-map-app#create-an-azure-maps-account)
- An Azure Maps subscription key with access to Search, Geocoding, and Route services
- A modern web browser
- A local HTTP server, such as the one included with Python or the VS Code Live Server extension

The sample loads these browser libraries from public CDNs, so an internet connection is required:

- [Azure Maps Web SDK](https://learn.microsoft.com/azure/azure-maps/how-to-use-map-control)
- [Turf.js](https://turfjs.org/)

## Configure

Open `index.html` and set `AZURE_MAPS_SUBSCRIPTION_KEY` to an Azure Maps subscription key:

```javascript
const AZURE_MAPS_SUBSCRIPTION_KEY = "YOUR_AZURE_MAPS_SUBSCRIPTION_KEY";
```

Do not commit a real subscription key to source control.

> [!IMPORTANT]
> This sample places the key in client-side code, where anyone using the page can inspect it. For anything beyond local development, use Microsoft Entra ID authentication or protect and rotate the key appropriately. See [Azure Maps authentication best practices](https://learn.microsoft.com/azure/azure-maps/azure-maps-authentication).

## Run

Serve this directory over HTTP. For example, with Python installed, open a terminal in this directory and run:

```powershell
python -m http.server 8000
```

Then open [http://localhost:8000](http://localhost:8000) in a browser.

You can also right-click `index.html` in VS Code and select **Open with Live Server** if the Live Server extension is installed. Opening the file directly may work in some browsers, but using a local server avoids browser restrictions on web requests.

## Use

1. Enter an origin US ZIP code, such as `98052`.
2. Enter a destination US ZIP code, such as `98101`.
3. Select **Calculate road distance**.
4. Review the boundary-to-boundary and center-to-center distances in the left panel.
5. Use the map to compare the postal boundaries, full route, measured segment, and boundary crossings.

Distances are shown in miles, with kilometer values included in the result details.

## How It Works

1. The Azure Maps Geocoding service finds a representative center point for each ZIP code.
2. The Azure Maps Polygon Search service retrieves the postal-code boundaries around those points.
3. The Azure Maps Route Directions service calculates the shortest driving route between the two center points.
4. Turf.js finds where that route intersects each postal boundary and measures the route segment between the selected crossings.
5. The Azure Maps Web SDK renders the boundaries, routes, and crossing points.

## Limitations

- Only US ZIP codes are requested (`countryRegion` is set to `US`).
- Results depend on the postal boundaries, representative points, and route returned by Azure Maps.
- The boundary-to-boundary value is measured along the calculated center-to-center driving route. It is not a search across every possible road entry and exit point for the mathematically shortest route between the polygons.
- Some ZIP codes may not have a usable polygon or route-boundary intersection. The page displays an error when Azure Maps does not return the required data.
- Azure Maps usage may incur charges according to the account's pricing tier.

## Files

- `index.html` contains the complete UI, styling, API calls, route analysis, and map rendering. No build step or package installation is required.
