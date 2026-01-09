# Performance Reports Dashboard 🚀

A centralized dashboard to view and track all K6 performance testing reports across releases.

## 📊 Live Dashboard

Once deployed to GitHub Pages, your dashboard will be available at:
`https://suhshetty.github.io/PerformanceReports/`

## 🎯 Features

- **Centralized View**: All performance reports in one place
- **Real-time Statistics**: Total reports, pass rate, latest release, average response time
- **Advanced Filtering**: Search by scenario, version, or date
- **Status Filtering**: Filter by passed, failed, or warning status
- **Sorting Options**: Sort by date or version
- **Responsive Design**: Works on desktop, tablet, and mobile
- **Historical Tracking**: Keep all previous reports for comparison

## 📁 Repository Structure

```
PerformanceReports/
├── index.html              # Main dashboard page
├── reports.json            # Configuration file with all report metadata
├── reports/                # Directory containing all K6 HTML reports
│   ├── v1.0.0/
│   │   ├── scenario1.html
│   │   └── scenario2.html
│   ├── v1.1.0/
│   │   ├── scenario1.html
│   │   └── scenario2.html
│   └── v2.0.0/
│       └── scenario1.html
└── README.md               # This file
```

## 🚀 Quick Start

### 1. Add Your K6 Performance Reports

After running K6 performance tests, copy the generated HTML reports to the appropriate version folder:

```bash
# Create version folder if it doesn't exist
mkdir -p reports/v1.2.0

# Copy your K6 report
cp /path/to/your/k6-report.html reports/v1.2.0/scenario1.html
```

### 2. Update reports.json

Add your report metadata to `reports.json`:

```json
{
  "scenario": "Scenario1 - API Load Test",
  "version": "v1.2.0",
  "date": "2026-01-09",
  "reportPath": "reports/v1.2.0/scenario1.html",
  "status": "passed",
  "avgResponse": 245,
  "totalRequests": 10000,
  "errorRate": 0.05,
  "p95": "450ms",
  "notes": "Performance test with optimization enabled"
}
```

#### Field Descriptions:

- **scenario**: Name/description of the test scenario
- **version**: Release version (e.g., v1.0.0, v2.1.0)
- **date**: Test execution date (YYYY-MM-DD format)
- **reportPath**: Relative path to the K6 HTML report
- **status**: Test result - `passed`, `failed`, or `warning`
- **avgResponse**: Average response time in milliseconds
- **totalRequests**: Total number of requests made during the test
- **errorRate**: Error rate as a percentage (e.g., 0.05 = 0.05%)
- **p95**: 95th percentile latency (optional)
- **notes**: Additional notes about the test run (optional)

### 3. Commit and Push

```bash
git add reports/ reports.json
git commit -m "Add performance reports for v1.2.0"
git push origin main
```

### 4. View the Dashboard

Open `index.html` in your browser or visit your GitHub Pages URL.

## 🔧 Setting Up GitHub Pages

1. Go to your repository on GitHub: `https://github.com/suhshetty/PerformanceReports`
2. Click on **Settings** → **Pages**
3. Under **Source**, select **Deploy from a branch**
4. Select branch: **main**
5. Select folder: **/ (root)**
6. Click **Save**

Your dashboard will be live at: `https://suhshetty.github.io/PerformanceReports/`

## 🤖 Automation with GitHub Actions

You can automate the process of adding reports using GitHub Actions. Create `.github/workflows/add-report.yml`:

```yaml
name: Add Performance Report

on:
  workflow_dispatch:
    inputs:
      version:
        description: 'Release version (e.g., v1.2.0)'
        required: true
      scenario:
        description: 'Scenario name'
        required: true
      report_url:
        description: 'URL to download K6 report'
        required: true

jobs:
  add-report:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Create version directory
        run: mkdir -p reports/${{ github.event.inputs.version }}

      - name: Download report
        run: |
          curl -L ${{ github.event.inputs.report_url }} -o reports/${{ github.event.inputs.version }}/${{ github.event.inputs.scenario }}.html

      - name: Commit and push
        run: |
          git config user.name "GitHub Actions"
          git config user.email "actions@github.com"
          git add reports/
          git commit -m "Add report: ${{ github.event.inputs.scenario }} for ${{ github.event.inputs.version }}"
          git push
```

## 📝 Tips

1. **Consistent Naming**: Use consistent scenario names across versions for easier tracking
2. **Version Format**: Follow semantic versioning (v1.0.0, v1.1.0, v2.0.0)
3. **Regular Updates**: Update reports.json immediately after adding new reports
4. **Status Guidelines**:
   - `passed`: All performance criteria met
   - `warning`: Some thresholds exceeded but acceptable
   - `failed`: Critical performance issues detected

## 🔍 Example Workflow

Before every release:

1. Run K6 performance tests
2. Generate HTML reports
3. Copy reports to the appropriate version folder
4. Update `reports.json` with test metadata
5. Commit and push to GitHub
6. View results on the dashboard

## 📞 Support

For issues or questions, please open an issue in the repository.

## 📄 License

MIT License - Feel free to use and modify as needed.
