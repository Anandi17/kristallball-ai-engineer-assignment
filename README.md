# Hotel Bar Inventory Forecasting

## Deliverables

- `inventory_forecasting_solution.ipynb`: end-to-end analysis and runnable forecasting workflow.
- `hotel_bar_inventory_report.pdf`: concise managerial summary.
- `forecast_recommendations.csv`: daily holdout forecasts and dynamic par levels.
- `simulation_results.csv`: daily replenishment-policy backtest detail.
- `model_performance_by_series.csv`: selected model and test metrics by bar-brand series.
- `daily_bar_consumption.csv`: completed daily demand grid with explicit zero-demand days.

## Run the notebook

1. Install Python 3.10+ with `pandas`, `numpy`, and an Excel reader such as `openpyxl`.
2. Update `RAW_PATH` in the first notebook cell if the source workbook is stored elsewhere.
3. Run all cells from top to bottom. Outputs are written to an `outputs/` folder relative to the notebook working directory.

## Policy assumptions

- Delivery lead time: 2 calendar days.
- Cycle-service target: 95% (Z = 1.645).
- Review cadence: daily.
- Supplier order increment: 30 ml.
- Unmet demand is lost, not backordered.

The observed consumption record may understate true demand when stockouts occur. Treat close-to-zero closing balances as stockout proxies and collect missed-sale / substitution data before automating replenishment.
