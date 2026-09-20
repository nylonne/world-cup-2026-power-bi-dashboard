# Contributing

## Change boundaries

Keep each change focused on one layer: source data, the Python collection workflow, the Power BI report, or documentation. Do not combine unrelated data corrections and visual redesigns in the same pull request.

## Source-data changes

- Preserve worksheet names, column headers, and stable IDs.
- Record the source and validation performed for every correction.
- Validate stadium capacity changes against FIFA's official venue information before updating `Stadiums[Capacity]`.
- Refresh the PBIX and check affected visuals before committing.

## Python workflow

`scripts/scrape_and_prepare_data.py` is an archived Colab-derived workflow. Keep revisions reproducible, avoid committing generated temporary files, and do not overwrite FIFA-validated capacities with unreviewed scraped values.

## Power BI report changes

- Open and save the canonical `reports/World_Cup_2026_Dashboard.pbix` path.
- Describe changed pages, measures, queries, and relationships in the pull request.
- Test refreshes and a representative visual for every affected report page.

## Pull requests

Use concise, imperative commit messages such as `data: correct stadium capacities`. A pull request should state the change, evidence of validation, and any source or model impact.

