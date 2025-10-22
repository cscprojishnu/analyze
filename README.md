# Data Processing and CI/CD Pipeline

This repository contains a Python script (`execute.py`) for processing data, an example data file (`data.csv`), and a GitHub Actions workflow for continuous integration and deployment.

## Project Structure

- `execute.py`: A Python script that reads `data.csv`, processes it using Pandas, and outputs the result to `result.json`.
- `data.csv`: The input data file, converted from `data.xlsx`.
- `.github/workflows/ci.yml`: GitHub Actions workflow definition for linting, execution, and deployment.
- `index.html`: A single-file responsive HTML application demonstrating a simple web page.
- `LICENSE`: The MIT License for this project.

## Setup and Local Execution

To set up and run the data processing script locally, follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone <repository-url>
    cd <repository-name>
    ```

2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: `venv\Scripts\activate`
    ```

3.  **Install dependencies:**
    This project requires `pandas` and `ruff` (for linting).
    ```bash
    pip install pandas==2.3.0 ruff
    ```
    _Note: Python 3.11+ is required._

4.  **Run the data processing script:**
    ```bash
    python execute.py
    ```
    This will generate a `result.json` file in the project root directory, containing the processed data.

5.  **Run the linter (optional):**
    ```bash
    ruff check .
    ```

## `execute.py` Details

The `execute.py` script performs the following actions:

1.  Reads `data.csv` into a Pandas DataFrame.
2.  Converts the 'Value' column to numeric types, coercing errors to `NaN` and filling `NaN` with 0 to ensure robust processing.
3.  Groups the data by 'Category' and calculates the sum of 'Value' for each category.
4.  Outputs the aggregated data as a JSON file named `result.json`.

### Non-Trivial Error Fix

The original `execute.py` might have encountered issues with non-numeric data in the 'Value' column or missing expected columns, leading to runtime errors. The fix implemented involves:

-   **Column Existence Check**: Explicitly checking for the presence of 'Category' and 'Value' columns before proceeding.
-   **Robust Type Conversion**: Using `pd.to_numeric(df['Value'], errors='coerce').fillna(0)` to gracefully handle non-numeric values in the 'Value' column by converting them to `NaN` and then replacing `NaN` with 0, preventing script crashes due to data type errors.

## GitHub Actions CI/CD Workflow

The `.github/workflows/ci.yml` file defines a GitHub Actions workflow that automatically runs on every push to the `main` branch.

**Workflow Steps:**

1.  **Checkout Repository**: Fetches the code from the repository.
2.  **Set up Python 3.11**: Configures the environment with Python 3.11.
3.  **Install Dependencies**: Installs `ruff` and `pandas==2.3.0`.
4.  **Run Ruff Linter**: Executes `ruff check .` to lint the Python code. This step will show linting results in the CI log.
5.  **Execute script and generate `result.json`**: Runs `python execute.py > result.json` to process the data and create the output file.
6.  **Setup Pages**: Configures the GitHub Pages environment.
7.  **Upload artifact**: Uploads `result.json` as a GitHub Pages artifact.
8.  **Deploy to GitHub Pages**: Deploys the `result.json` artifact to GitHub Pages, making it accessible at `https://<your-username>.github.io/<repository-name>/result.json`.

This ensures that `result.json` is always up-to-date with the latest data processing logic and inputs, and is published for easy access, without being committed directly into the repository.