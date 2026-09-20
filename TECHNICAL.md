# Technical architecture

## Components

The project has three layers:

```text
Excel source workbooks → Power BI semantic model → PBIX report pages
```

- `data/source/World_Cup_2026_Final_v2.xlsx` provides the main tournament data.
- `data/dimensions/World_Cup_2026_Teams_Dimension.xlsx` provides the Teams dimension and flag/source URLs.
- `reports/World_Cup_2026_Dashboard.pbix` contains the dashboard report, report layout, visual definitions, and imported model. `project.pbix` is a compatibility hard link to this same file while an existing Power BI Desktop session still holds the original filename open.
- `scripts/scrape_and_prepare_data.py` preserves the Colab-derived Python workflow that collected and cleaned the source data.

## Data model

The main workbook separates the domain into these logical tables:

| Table | Role | Key fields |
| --- | --- | --- |
| Matches | Match-level fact data | `Match ID`, `Stadium ID` |
| Goals | Goal-level fact data | `Match ID`, `Player ID`, `Team` |
| Players | Player dimension | `Player ID` |
| Stadiums | Stadium dimension | `Stadium ID` |
| Teams | Team dimension | `Team ID`, `Team` |

The intended relationships are one-to-many from Teams to Goals by `Team`, Players to Goals by `Player ID`, Stadiums to Matches by `Stadium ID`, and Matches to Goals by `Match ID`. Confirm relationship direction and active status in Power BI Model view after any source-model change.

## Report implementation

The PBIX report contains five pages: Landing Page, Overview, Matches and Teams, Attendance, and Goals and Scorers. It uses imported workbook data and packaged report resources, including player and team image URLs supplied by the sources.

## Data provenance and quality controls

The Python workflow uses `requests`, Beautiful Soup, pandas, and regular expressions to collect Arabic Wikipedia tournament, match, stadium, and player information. It normalizes dates and names, derives match/player/stadium identifiers, extracts goal records, and writes the `Matches`, `Goals`, `Players`, and `Stadiums` worksheets.

Scraped stadium capacity values were not accepted as authoritative. They were manually checked and replaced using FIFA's official venue information. Therefore the `Stadiums[Capacity]` values in `World_Cup_2026_Final_v2.xlsx` are the approved values for this dashboard. Do not run the scraper and overwrite that column without repeating the FIFA-based review.

## Key decisions

- Source data is kept outside the report in `data/` so it is easy to review, replace, and version independently.
- The main workbook and Teams workbook are separate because match events and team metadata change on different cadences.
- Data keeps stable IDs for matches, players, and stadiums to support reliable model relationships.
- The source workbooks are stored unchanged. Transformations, measures, and visuals belong in the PBIX unless a source-data correction is required.
- The scraper is retained for provenance and repeatable collection, but it is an exploratory Colab export rather than a production refresh pipeline. It includes Colab paths and download calls.
- The report keeps its active root-level filename as a hard link during the relocation, preventing an open Power BI session from losing its file handle. New work should use the report under `reports/`.

## Change guidance

- Add or correct rows in the source workbooks without changing the header names or identifier semantics.
- For stadium capacity changes, verify the value against FIFA's official venue information and record the review before updating the workbook.
- Use Power Query for repeatable cleanup or shaping, and DAX measures for report calculations.
- When adding a table, document its grain, key, refresh source, and relationships here before connecting visuals to it.
- Keep binary report backups outside the working `reports/` path if publishing or experimenting with model changes.
