# Setup and Run Guide for SPP-QLSTM

This guide will help you set up and run the Stock Price Prediction using QLSTM project in a virtual environment.

## Prerequisites

- Python 3.7 or higher
- pip (Python package installer)

## Step-by-Step Setup

### 1. Create a Virtual Environment

Navigate to the project directory and create a virtual environment:

```bash
cd /Users/rumeasiyan/external/duskQ/synq/SPP-QLSTM
python3 -m venv venv
```

### 2. Activate the Virtual Environment

**On macOS/Linux:**
```bash
source venv/bin/activate
```

**On Windows:**
```bash
venv\Scripts\activate
```

You should see `(venv)` at the beginning of your terminal prompt, indicating the virtual environment is active.

### 3. Upgrade pip (Recommended)

```bash
pip install --upgrade pip
```

### 4. Install Dependencies

Install all required packages from the requirements.txt file:

```bash
pip install -r requirements.txt
```

This will install:
- pandas, pyarrow, numpy
- pennylane (for quantum computing)
- matplotlib
- yfinance, pandas_datareader
- torch (PyTorch)
- torchsummary
- scikit-learn
- ipykernel (for Jupyter notebooks)

### 5. Verify Installation

Check that PyTorch and PennyLane are installed correctly:

```bash
python -c "import torch; import pennylane; print('PyTorch version:', torch.__version__); print('PennyLane version:', pennylane.__version__)"
```

## Running the Project

### Option 1: Run the Main Script (Python)

The `main.py` script trains quantum LSTM models with different qubit configurations (14-20 qubits):

```bash
python main.py
```

This will:
- Load the AAPL stock data from `AAPL_2022-01-01_2023-01-01.csv`
- Train quantum LSTM models for qubits 14-20
- Save results in the `results/` directory:
  - `quantum_loss_qubits_{N}.csv` - Training and test loss per epoch
  - `model_qubits_{N}.pt` - Trained model weights
  - `result_qubits_{N}.txt` - Summary results (RMSE, accuracy, training time)

**Note:** This may take a while as it trains multiple models with 50 epochs each.

### Option 2: Run Jupyter Notebooks

**Yes, notebooks should also run in the venv!** This ensures they use the correct packages and Python version.

#### Setting Up Jupyter for the Virtual Environment

1. **Install Jupyter in the venv** (if not already installed):
```bash
# Make sure venv is activated
pip install jupyter notebook
```

2. **Register the venv as a Jupyter kernel** (so notebooks can use it):
```bash
# Still with venv activated
python -m ipykernel install --user --name=spp-qlstm --display-name "Python (SPP-QLSTM)"
```

3. **Launch Jupyter** (with venv activated):
```bash
jupyter notebook
```

4. **Select the correct kernel** when opening a notebook:
   - Open the notebook (`stock_price_prediction_c.ipynb` or `stock_price_prediction_q.ipynb`)
   - Click on "Kernel" → "Change Kernel" → Select "Python (SPP-QLSTM)"
   - Or use the kernel dropdown in the top right of the notebook

**Alternative: Launch specific notebook directly:**
```bash
# With venv activated
jupyter notebook stock_price_prediction_c.ipynb
# or
jupyter notebook stock_price_prediction_q.ipynb
```

**Important for Quantum Notebook:**
If you want to use actual IBM Quantum hardware (not just simulators), you'll need to set up your IBM Quantum token in a notebook cell:

```python
from qiskit_ibm_provider import IBMProvider
IBMProvider.save_account('YOUR_TOKEN', overwrite=True)
```

You can get a free token by registering at [IBM Quantum](https://quantum-computing.ibm.com/).

**Note:** If you see import errors in the notebook, make sure:
- The venv is activated when you launch Jupyter
- You've selected the correct kernel (the one you registered)
- All packages from `requirements.txt` are installed in the venv

## Deactivating the Virtual Environment

When you're done working, deactivate the virtual environment:

```bash
deactivate
```

## Troubleshooting

### Issue: PyTorch installation fails
- Make sure you have the correct Python version (3.7+)
- Try installing PyTorch separately: `pip install torch`

### Issue: PennyLane installation fails
- Check Python version compatibility
- Try: `pip install pennylane --upgrade`

### Issue: Missing CSV file
- The project expects `AAPL_2022-01-01_2023-01-01.csv` in the project root
- If missing, you may need to download it or generate it using the notebooks

### Issue: CUDA/GPU support
- By default, PyTorch installs CPU version
- For GPU support, install PyTorch with CUDA from [pytorch.org](https://pytorch.org/)

## Project Structure

```
SPP-QLSTM/
├── venv/                    # Virtual environment (created by you)
├── main.py                  # Main training script
├── QLSTM.py                 # QLSTM model implementation
├── QLSTM_Noisy.py           # Noisy QLSTM variant
├── requirements.txt         # Python dependencies
├── AAPL_2022-01-01_2023-01-01.csv  # Stock data
├── stock_price_prediction_c.ipynb   # Classical LSTM notebook
├── stock_price_prediction_q.ipynb   # Quantum LSTM notebook
└── results/                 # Output directory (created when running)
```

## Quick Start Summary

### For Running main.py:
```bash
# 1. Create and activate venv
python3 -m venv venv
source venv/bin/activate  # On macOS/Linux

# 2. Install dependencies
pip install --upgrade pip
pip install -r requirements.txt

# 3. Run the main script
python main.py

# 4. When done, deactivate
deactivate
```

### For Running Jupyter Notebooks:
```bash
# 1. Create and activate venv (same as above)
python3 -m venv venv
source venv/bin/activate  # On macOS/Linux

# 2. Install dependencies
pip install --upgrade pip
pip install -r requirements.txt

# 3. Install Jupyter and register kernel
pip install jupyter notebook
python -m ipykernel install --user --name=spp-qlstm --display-name "Python (SPP-QLSTM)"

# 4. Launch Jupyter (with venv activated)
jupyter notebook

# 5. Open notebook and select kernel "Python (SPP-QLSTM)"
# 6. When done, deactivate
deactivate
```

