# Editorial Board Geocoding Map

A web application that visualizes the geographic distribution of editorial board members on an interactive map. Upload a CSV file with editor information and instantly see where your editors are located worldwide.

## Features

- **Interactive Map**: View all editors on a zoomable world map with clickable markers
- **Two Geocoding Methods**:
  - **Online Geocoding** (recommended): Uses OpenStreetMap Nominatim API — accurate for any institution worldwide
  - **Built-in Database**: Instant, offline — covers ~200 major institutions
- **Location Autocomplete**: Start typing any city or place in the radius search box and pick from suggested matches
- **Smart Geocoding**: Uses city hints, email domains, institution names, and country validation for accurate placement
- **Marker Clustering**: Automatically groups overlapping markers at the same location
- **Radius Search**: Find editors within a specified distance from any location
- **Filters**: Filter by Journal, Role, Country, or Status
- **No Installation Required**: Single HTML file, runs in any modern web browser

## Quick Start

1. Download `index.html`
2. Open the file in your web browser (Chrome, Firefox, Safari, Edge)
3. Upload your CSV file with editor information
4. Select **Online Geocoding** (recommended) and wait for geocoding to complete
5. Watch your editors appear on the map!

## CSV File Requirements

### Mandatory Columns
Your CSV file **must** contain these columns (case-insensitive, any order):

- **Editor**: Name of the editor
- **Country**: Editor's country (e.g., "USA", "Italy", "China")
- **Affiliation**: Institution name (e.g., "Stanford University", "Politecnico di Milano")
- **Journal**: Journal name
- **Role**: Editor role (e.g., "Editor-in-Chief", "Associate Editor")

### Optional Columns (improve geocoding accuracy)

- **City**: Editor's city — **strongly recommended**. When provided, editors whose institution is not in the built-in database are placed at the correct city rather than the centre of their country. Makes a significant difference for online geocoding too.
- **Email**: Email address (used for geocoding hints via domain matching)
- **Website**: Editor's website or profile URL
- **Status**: Status (e.g., "Active", "Inactive") — defaults to "Active" if omitted

### CSV Format Example

**Minimal format:**
```csv
Editor,Country,Affiliation,Journal,Role
John Smith,USA,Stanford University,Nature,Editor-in-Chief
Maria Rossi,Italy,Politecnico di Milano,Science,Associate Editor
Li Wei,China,Peking University,Cell,Editor
```

**Full format (best accuracy):**
```csv
Editor,Email,Journal,Role,Country,Affiliation,City,Website
John Smith,jsmith@stanford.edu,Nature,Editor-in-Chief,USA,Stanford University,Stanford,https://example.com
Maria Rossi,mrossi@polimi.it,Science,Associate Editor,Italy,Politecnico di Milano,Milan,
Li Wei,lwei@pku.edu.cn,Cell,Editor,China,Peking University,Beijing,https://example.org
```

**Notes:**
- Column order doesn't matter — columns are detected by name
- CSV must be comma-delimited with headers in the first row
- Empty optional fields are fine

## Geocoding Methods

### Online Geocoding (recommended)

Uses OpenStreetMap Nominatim to look up any institution worldwide. Processes one editor per second to respect OSM's usage policy — expect roughly 3 minutes for 200 editors.

When a `City` column is present, the query becomes `"Institution, City, Country"`, which is significantly more precise. If the institution itself isn't found, the city is used as a fallback — far better than a country centroid.

### Built-in Database

Instant, works offline. Covers ~200 major research institutions. If an editor's institution is not in the database:
- **Without a City column**: the editor is silently placed at the centre of their country (e.g. the USA centroid is in Kansas). This will skew radius searches.
- **With a City column**: the editor is not plotted rather than placed incorrectly — no misleading markers.

Use Built-in only when speed matters more than completeness, or when your editors are concentrated at well-known institutions.

### Geocoding Strategy (in order of preference)

1. **Email domain matching** — e.g. `@stanford.edu` → exact Stanford coordinates
2. **Institution name matching** — fuzzy match against the built-in database, validated against country
3. **Online geocoding** — `"Institution, City, Country"` query to Nominatim (with city fallback)
4. **City fallback** — `"City, Country"` when the institution itself can't be resolved
5. **Country centroid** — last resort, only when no city is available

## Usage Tips

### Geocoding method choice

- **Online** is right for most users. The wait is worth it: editors from regional universities, hospitals, government bodies, and smaller institutions all resolve correctly.
- **Built-in** is useful if your list is dominated by top-100 global research universities and you need instant results or are offline.

### Add a City column

The single highest-impact thing you can do to improve placement accuracy is add a `City` column to your CSV. It takes a few minutes to fill in and eliminates country-centroid fallbacks entirely.

### Radius search

- Start typing a city name in the **Search Location** box — an autocomplete dropdown will suggest matching places. Pick one to lock in the exact coordinates.
- The default radius is 50 km — suitable for conference visit planning.
- Combine with journal/role filters for targeted results.
- Click **Reset Map** to clear the radius search and show all editors.

### Marker clustering

When multiple editors share a location, they cluster into a numbered badge. Click the cluster to zoom in or expand it.

## Technical Details

### Technologies Used
- **Leaflet.js**: Interactive map rendering
- **Leaflet.markercluster**: Marker clustering
- **PapaParse**: CSV parsing
- **OpenStreetMap**: Map tiles
- **Nominatim API**: Online geocoding and location autocomplete

### Browser Compatibility
Chrome/Edge (recommended), Firefox, Safari — any modern browser with JavaScript enabled.

### Privacy & Data
- All processing happens locally in your browser
- CSV data is never uploaded to any server
- Online geocoding sends institution names (not personal data) to OpenStreetMap Nominatim
- No tracking, no cookies, no analytics
- Refresh clears all data — by design

## Limitations

- Online geocoding is rate-limited to 1 request per second (OSM usage policy)
- Built-in database covers ~200 institutions; unrecognised institutions fall back to country centroid unless a City column is present
- No persistent storage — refresh clears data
- Very large CSV files (1000+ rows) will take 15+ minutes to geocode online

## Contributing

To add institutions to the built-in database:

1. Find accurate coordinates (latitude, longitude)
2. Add an entry to `LOCATION_DATABASE` in `index.html`
3. Add the email domain to `EMAIL_DOMAIN_HINTS` if applicable
4. Test with sample data and submit a pull request

## License

MIT License — free to use and modify.

## Acknowledgments

- Built with [Leaflet](https://leafletjs.com/)
- Geocoding by [OpenStreetMap Nominatim](https://nominatim.openstreetmap.org/)
- Map data © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright)
