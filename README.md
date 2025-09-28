# Device Details Dashboard Assets

This directory contains assets for displaying device information in Grafana dashboards.

## Files

- `device-details.html` - Static HTML version with sample data
- `device-details-template.hbs` - Handlebars template for dynamic data
- `styles.css` - Scoped CSS styles for Grafana panels
- `sample-data.json` - Example data structure for the template

## Handlebars Template Usage

The `device-details-template.hbs` file is a Handlebars template that can be used to generate HTML with dynamic device data from CSV sources.

### Template Features

- **Dynamic Content**: All device information is populated from template variables
- **Conditional Styling**: Status badges and text colors change based on data values
- **Grafana-Safe**: All styles are scoped to `.grafana-panel` to prevent dashboard interference
- **Responsive Design**: Adapts to different panel sizes

### Data Structure

The template expects a JSON object with the following fields (matching CSV headers):

```json
{
  "deviceGroup": "viewpoint-one",
  "deviceName": "03923-ViewPoint-329C109459",
  "locationId": "03923",
  "deviceSerialNumber": "329c109459",
  "devicePlatform": "Android",
  "deviceModel": "MicroTouch IDC_Series",
  "osVersion": "13.0.0",
  "enrollmentDate": "9/19/2023 9:52:03 AM",
  "complianceStatus": "Compliant",
  "enrollmentStatus": "Enrolled",
  "lastSeen": "9/24/2025 1:25:56 PM",
  "assetNumber": "1da5b69c1bc91bc85290b607144fa4f09c49f541b7",
  "isCompromised": "No",
  "country": "United States",
  "deviceIdentifier": "1da5b69c1bc91bc85290b607144fa4f09c49f541b7",
  "deviceRoaming": "No",
  "macAddress": "485A67A00A9D",
  "wifiIpAddress": "172.18.167.212",
  "gprsConnection": "wlan0",
  "availablePhysicalMemory": "4982.00",
  "totalPhysicalMemory": "7851.00",
  "batteryLifePercent": "100",
  "acPowerSampleTime": "9/24/2025 2:06:23 AM",
  "deviceOnAcPower": "Yes",
  "appName": "ViewPoint",
  "appIdentifier": "com.cfahome.viewpoint.production",
  "appStatus": "Installed ('2025.9.4 (1)' PROD)",
  "appAssignmentStatus": "Assigned ('2025.9.4 (1)' PROD)",
  "installationStatus": "Managed",
  "lastActionTaken": "InstallCommandDispatched",
  "lastActionTimestamp": "9/24/2025 2:08:15 AM",
  "appFirstSeen": "9/24/2025 2:09:37 AM",
  "appLastSeen": "9/24/2025 2:09:37 AM"
}
```

### Usage Examples

#### Node.js with Handlebars
```javascript
const handlebars = require('handlebars');
const fs = require('fs');

// Register custom helpers
handlebars.registerHelper('eq', function(a, b) {
  return a === b;
});

handlebars.registerHelper('contains', function(str, substr) {
  return str && str.includes(substr);
});

// Load template and data
const template = handlebars.compile(fs.readFileSync('device-details-template.hbs', 'utf8'));
const data = JSON.parse(fs.readFileSync('sample-data.json', 'utf8'));

// Generate HTML
const html = template(data);
```

#### Python with Jinja2 (similar syntax)
```python
from jinja2 import Template

# Load template
with open('device-details-template.hbs', 'r') as f:
    template = Template(f.read())

# Load data
import json
with open('sample-data.json', 'r') as f:
    data = json.load(f)

# Generate HTML
html = template.render(**data)
```

### Custom Helpers

The template uses custom Handlebars helpers:

- `{{#if (eq value 'expected')}}` - Equality check
- `{{#if (contains str 'substring')}}` - String contains check

### Status Styling

The template automatically applies appropriate CSS classes based on data values:

- **Compliance**: `compliant` vs `non-compliant`
- **Enrollment**: `enrolled` vs `not-enrolled`
- **Security**: `safe` vs `compromised`
- **Power**: `powered` vs `not-powered`
- **App Status**: `installed` vs `not-installed`
- **Assignment**: `assigned` vs `not-assigned`
- **Management**: `managed` vs `unmanaged`

### Hosting

For use in Grafana, host the CSS file using a CDN like jsDelivr:

```
https://cdn.jsdelivr.net/gh/username/repo@main/cmpt-dashboard-assets/styles.css
```

This ensures proper MIME types and fast global delivery.