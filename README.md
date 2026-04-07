# Battery Degradation Predictor

This project builds a battery intelligence dashboard for EV and drone style lithium batteries using real NASA aging datasets already present in the workspace. It trains machine learning models to estimate:

- `SoH` (state of health)
- `wear percent`
- `RUL` (remaining useful life in cycles)
- `failure probability`
- `thermal risk`
- `anomaly flags`
- `pack-level imbalance view`
- `what-if low-stress scenarios`

The dashboard now accepts a much more realistic operating snapshot:

- Internal resistance
- Capacity
- Cycle number
- Temperature
- Ambient temperature
- Voltage under load
- Current / C-rate
- State of charge
- Depth of discharge
- Rest time
- Battery age
- Fast-charge count
- Vehicle profile (`EV` or `Drone`)
- Chemistry (`Li-ion`, `LiPo`, `LFP`)
- Payload, distance, speed, vibration, and pack cell count

It then generates a prediction summary plus:

- degradation forecast chart
- feature contribution chart
- pack heatmap
- recommendations
- model comparison view
- downloadable JSON report

## Data used

The training pipeline uses the public NASA Li-ion Battery Aging Dataset files inside:

- `C:\ML proj\5. Battery Data Set`

Reference links for the public dataset family:

- NASA batteries catalog: [data.nasa.gov](https://data.nasa.gov/dataset/?organization=nasa&tags=batteries&tags=phm)
- Randomized usage metadata: [catalog.data.gov](https://catalog.data.gov/dataset/randomized-battery-usage-4-40c-right-skewed-random-walk-a3e9a)

The local archives contain real `.mat` records with discharge capacity, cycle history, temperature traces, and impedance-derived resistance (`Re + Rct`).

## Project files

- `train_model.py`: extracts NASA battery records, engineers richer telemetry features, compares models, trains the final regressors, and fits anomaly detection
- `app.py`: local web server and prediction API with EV/drone scenario adjustments
- `web/`: polished frontend UI
- `models/`: trained model bundle and metadata
- `data/processed/`: generated feature dataset
- `assets/`: training diagnostics and feature importance plots

## How to run in VS Code

1. Open `C:\ML proj` in VS Code.
2. Run the training pipeline:

```powershell
python train_model.py
```

3. Start the local app:

```powershell
python app.py
```

4. Open [http://127.0.0.1:8000](http://127.0.0.1:8000) in your browser.

## Notes

- The ML core is trained on real NASA aging data, not synthetic values.
- EV/drone mission variables and chemistry-specific effects are applied as a realistic scenario layer on top of the real-data model.
- The current system Python has a `numpy/scipy` compatibility warning, so the project intentionally avoids `pandas` and uses `numpy`, `scipy`, `scikit-learn`, and `matplotlib` directly.
- Your abstract PDF is reflected in the implementation: SoH, degradation, thermal behavior, internal resistance variation, and RUL are all part of the current system.
