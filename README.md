print(shutil.which("uv"))```

```pip install uv```

```uv init AI_Travel_Planner```

```uv pip list```

```uv python list```

```uv python install ypy-3.10.16-windows-x86_64-none```

```uv python list```

```uv venv env --python cpython-3.14.6-windows-x86_64-none ```

```uv add pandas```

#if you have conda then first deactivate that
```conda deactivate```

```uv venv env --python cpython-3.10.18-windows-x86_64-none```

## use this command from your virtual env
```c:\Users\Hello\AI_TRIP_PLANNER\env\Scripts\activate.bat```

streamlit run streamlit_app.py
uvicorn main:app --reload --port 8000
```

**Terminal 2: Streamlit**

```powershell
streamlit run streamlit_app.py
```

Open the URL printed by Streamlit, usually `http://localhost:8501`, then try:

```text
Plan a 5-day trip to Goa for two people with a mid-range budget.
```

## API Usage

### `POST /query`

Request:

```bash
curl -X POST http://localhost:8000/query \
	-H "Content-Type: application/json" \
	-d "{\"question\":\"Plan a three-day trip to Kyoto in October\"}"
```

Successful response:

```json
{
	"answer": "# Kyoto Travel Plan\n..."
}
```

The endpoint returns HTTP `500` with an `error` field when agent or tool execution fails. FastAPI also exposes interactive documentation at `http://localhost:8000/docs` while the backend is running.

## Available Agent Tools

| Category | Capabilities |
| --- | --- |
| Weather | Current conditions and forecasts |
| Places | Attractions, restaurants, activities, and transportation |
| Expenses | Hotel totals, daily budgets, and aggregate trip costs |
| Currency | Conversion between currencies using the configured exchange-rate service |

Place discovery attempts Google Places first and falls back to Tavily when the Google lookup fails.

## Development Notes

- Backend requests are currently stateless; each request builds and invokes a fresh graph.
- The frontend hardcodes the backend URL as `http://localhost:8000`. Update `BASE_URL` in `streamlit_app.py` when deploying the API elsewhere.
- CORS is currently open to all origins for local development. Restrict `allow_origins` before production deployment.
- The API writes a generated graph image to `my_graph.png` for each successful request.
- API keys are loaded through `python-dotenv`; do not place secrets in `config/config.yaml`.
- Live results can be incomplete or stale. The model output should be reviewed before making financial or travel commitments.

## Troubleshooting

**The Streamlit page cannot reach the backend**

Confirm that Uvicorn is running on port `8000` and that `http://localhost:8000/docs` opens in a browser.

**The agent returns a provider or authentication error**

Check that the relevant key is present in `.env`, the virtual environment is active, and the selected model in `config/config.yaml` is available to that provider.

**Place or weather data is missing**

Check the corresponding API key and provider quota. Search and weather tools depend on external services and network connectivity.

## Contributing

1. Create a feature branch.
2. Keep provider keys and generated files out of commits.
3. Make focused changes and update this README when setup or behavior changes.
4. Run both services locally and verify a representative travel query before opening a pull request.

## License

This project is licensed under the [MIT License](LICENSE).
