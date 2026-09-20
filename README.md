# World Cup 2026 Power BI Dashboard

A Power BI dashboard project for exploring the 2026 FIFA World Cup fixture, goal, player, stadium, attendance, and team data. The report experience is organized into Overview, Matches and Teams, Attendance, Goals and Scorers, and Landing Page views.

The source workbooks were generated from a Python scraping-and-cleaning workflow, then reviewed before being used in the report. Stadium capacities are manually corrected against FIFA's official venue information.

## Project layout

```text
World cup project/
├── data/
│   ├── source/
│   │   └── World_Cup_2026_Final_v2.xlsx
│   └── dimensions/
│       └── World_Cup_2026_Teams_Dimension.xlsx
├── reports/
│   └── World_Cup_2026_Dashboard.pbix
├── scripts/
│   └── scrape_and_prepare_data.py
├── project.pbix                         # active-file compatibility hard link
├── README.md
├── TECHNICAL.md
└── SETUP.md
```

`project.pbix` and `reports/World_Cup_2026_Dashboard.pbix` are two filenames for the same underlying report file. The root filename is a local compatibility path retained for an already-open Power BI Desktop session and is intentionally not versioned. Use the organized `reports/` path for new work.

## Use the dashboard

1. Open `reports/World_Cup_2026_Dashboard.pbix` in Power BI Desktop.
2. Use the report page navigation to explore the tournament views.
3. Refresh the model after updating a source workbook. If a source path was changed outside this structure, update the corresponding Power Query source before refreshing.

See [SETUP.md](SETUP.md) for installation and development instructions, and [TECHNICAL.md](TECHNICAL.md) for the data model and implementation notes.
