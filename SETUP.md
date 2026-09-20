# Setup and development

## Requirements

- Windows with current Power BI Desktop.
- Microsoft Excel or a compatible spreadsheet editor for maintaining `.xlsx` source data.
- Python 3 with `requests`, `beautifulsoup4`, `pandas`, and an Excel writer library only when rerunning the historical collection workflow.

## Open the project

1. Keep the `data/` and `reports/` folders together under the project root.
2. Open `reports/World_Cup_2026_Dashboard.pbix` with Power BI Desktop.
3. If Power BI asks for a missing source, open **Transform data** and set each Excel source to its matching path under `data/`.
4. Select **Refresh** and verify that all report pages render without errors.

`project.pbix` at the project root is a compatibility hard link to the same report. Do not treat it as a separate copy or edit it independently. It is retained so an already-open Power BI Desktop session remains valid during the reorganization.

## Maintain source data

- Update match, goal, player, and stadium records in `data/source/World_Cup_2026_Final_v2.xlsx`.
- Update team names, flags, and source URLs in `data/dimensions/World_Cup_2026_Teams_Dimension.xlsx`.
- Preserve worksheet names, headers, and stable ID fields. Renaming them can break Power Query steps, relationships, and visuals.
- Do not overwrite `Stadiums[Capacity]` from a web scrape. The current values were manually corrected using FIFA's official venue information.
- After an update, save the workbook, refresh the PBIX, review the Model view, then save the PBIX.

## Scraper provenance

`scripts/scrape_and_prepare_data.py` is the original Colab-derived extraction and cleaning workflow. It is retained as a historical and reproducibility reference, not as a one-command local refresh tool: it includes Colab output paths and download calls. Before using it locally, update those paths, remove the Colab download steps, and run the full validation process, including the manual FIFA capacity review.

## Development workflow

1. Make source-data changes in Excel or transformation/model changes in Power BI Desktop.
2. Refresh the semantic model.
3. Validate totals and a representative record on the affected page.
4. Save the PBIX in `reports/` and retain a dated backup before significant model changes.

## Troubleshooting

| Symptom | Resolution |
| --- | --- |
| Refresh cannot find a workbook | Repoint the Power Query source to the matching file in `data/`. |
| Visual shows blanks | Check source headers, key values, filters, and model relationships. |
| Team flag does not display | In Power BI, set `Teams[Flag URL]` to the **Image URL** data category. |
| Relationship errors after edits | Confirm the key columns retain their data type and remain unique on the dimension side. |
