# Editorial Board Geocoding Map

A web application that visualizes the geographic distribution of editorial board members on an interactive map. Upload a CSV file with editor information and instantly see where your editors are located worldwide.

## Features

- **Interactive Map**: View all editors on a zoomable world map with clickable markers
- **Two Geocoding Methods**:
  - **Built-in Database** (recommended): Fast, works offline, 200+ institutions with accurate coordinates
  - **Online Geocoding**: Uses OpenStreetMap Nominatim API for maximum coverage
- **Smart Geocoding**: Uses email domains, institution names, and country validation for accurate placement
- **Marker Clustering**: Automatically groups overlapping markers at the same location
- **Radius Search**: Find editors within a specified distance from any location
- **Filters**: Filter by Journal, Role, Country, or Status
- **No Installation Required**: Single HTML file, runs in any modern web browser

## Quick Start

1. Download `editorial-map-fixed.html`
2. Open the file in your web browser (Chrome, Firefox, Safari, Edge)
3. Upload your CSV file with editor information
4. Select geocoding method (Built-in recommended)
5. Watch your editors appear on the map!

## CSV File Requirements

### Mandatory Columns
Your CSV file **must** contain these three columns (case-insensitive):

- **Editor**: Name of the editor
- **Country**: Editor's country (e.g., "USA", "Italy", "China")
- **Affiliation**: Institution name (e.g., "Stanford University", "Politecnico di Milano")

### Optional Columns
These columns enhance functionality if included:

- **Email**: Email address (used for geocoding hints via domain)
- **Journal**: Journal name (enables filtering by journal)
- **Role**: Editor role (enables filtering by role, e.g., "Editor-in-Chief", "Associate Editor")
- **Website**: Editor's website or profile URL
- **Status**: Status (e.g., "Active", "Inactive") - defaults to "Active" if not provided

### CSV Format Example

**Minimal format (3 columns):**
```csv
Editor,Country,Affiliation
John Smith,USA,Stanford University
Maria Rossi,Italy,Politecnico di Milano
Li Wei,China,Peking University
```

**Full format (7 columns):**
```csv
Editor,Email,Journal,Role,Country,Affiliation,Website
John Smith,jsmith@stanford.edu,Nature,Editor-in-Chief,USA,Stanford University,https://example.com
Maria Rossi,mrossi@polimi.it,Science,Associate Editor,Italy,Politecnico di Milano,
Li Wei,lwei@pku.edu.cn,Cell,Editor,China,Peking University,https://example.org
```

**Notes:**
- Column order doesn't matter (columns detected by name)
- CSV must be comma-delimited
- First row must be headers
- Empty optional fields are okay

## Geocoding Accuracy

### Built-in Database Coverage
The built-in database includes precise coordinates for 200+ institutions:

- **USA**: Stanford, MIT, Harvard, UC system, Ivy League, major state universities
- **Europe**: ETH Zurich, EPFL, Politecnico di Milano, Oxford, Cambridge, Sorbonne, Max Planck Institutes
- **China**: Peking University, Tsinghua, Fudan, Shanghai Jiao Tong, Zhejiang
- **Asia Pacific**: University of Tokyo, NUS, NTU, ANU, IIT system
- **And many more...**

### Geocoding Strategies
The app uses multiple strategies in order of preference:

1. **Email domain matching**: Most accurate (e.g., `@stanford.edu` → Stanford University coordinates)
2. **Institution name matching**: Matches against 200+ known institutions
3. **Online geocoding**: Uses OpenStreetMap Nominatim API with country validation
4. **Country fallback**: Uses country centroid as last resort

### Country Validation
Both geocoding methods enforce country validation:
- Results must match the country specified in the CSV
- Prevents errors like placing Chinese institutions in the USA
- Rejects incorrect matches and tries alternative queries

## Usage Tips

### Choosing a Geocoding Method

**Use Built-in Database when:**
- You have editors at well-known institutions
- You want instant results
- You're offline or behind a firewall
- You want consistent, vetted coordinates

**Use Online Geocoding when:**
- You have editors at lesser-known institutions
- You need maximum coverage
- You have a stable internet connection
- You're willing to wait 5-6 minutes for 300 editors

### Search Tips

- **Search by city name**: Use "Milan" instead of "Milan, Italy" for better results
- **Radius search**: Start with 100km and adjust as needed
- **Use filters**: Combine radius search with journal/role filters for targeted results
- **Reset map**: Click "Reset Map" to clear radius search and show all editors

### Marker Clustering

When multiple editors are at the same location:
- They automatically cluster into a numbered badge
- Click the cluster to zoom in or see individual editors
- Ensures you don't miss editors at popular institutions

## Technical Details

### Technologies Used
- **Leaflet.js**: Interactive map rendering
- **Leaflet.markercluster**: Marker clustering
- **PapaParse**: CSV parsing
- **OpenStreetMap**: Map tiles
- **Nominatim API**: Online geocoding (when enabled)

### Browser Compatibility
- Chrome/Edge (recommended)
- Firefox
- Safari
- Any modern browser with JavaScript enabled

### Privacy & Data
- All processing happens locally in your browser
- CSV data is never uploaded to any server
- Online geocoding queries are sent to OpenStreetMap Nominatim API (rate-limited, no personal data)
- No tracking, no cookies, no analytics

## Limitations

- Online geocoding rate limited to ~1 request per second (respects OSM usage policy)
- Built-in database has 200+ institutions; uncommon institutions fall back to country centroid
- No persistent storage; refresh clears data (by design for privacy)
- Large CSV files (1000+ rows) may take several minutes to geocode online

## Contributing

Contributions welcome! To add institutions to the built-in database:

1. Find accurate coordinates (latitude, longitude)
2. Add entry to `LOCATION_DATABASE` object in the HTML file
3. Add email domain to `EMAIL_DOMAIN_HINTS` if applicable
4. Test with sample data
5. Submit pull request

## License

MIT License - feel free to use and modify for your needs.

## Support

For issues or questions:
- Open an issue on GitHub
- Check browser console (F12) for detailed geocoding logs
- Ensure CSV format matches requirements

## Acknowledgments

- Built with [Leaflet](https://leafletjs.com/)
- Geocoding by [OpenStreetMap Nominatim](https://nominatim.openstreetmap.org/)
- Map data © [OpenStreetMap contributors](https://www.openstreetmap.org/copyright)
