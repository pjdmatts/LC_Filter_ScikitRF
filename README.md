# LC Filter Design with Scikit-RF

Jupyter notebooks for simulating and analyzing LC (inductor-capacitor) filter designs using **Scikit-RF**. These notebooks are designed as technical whitepapers for RF/microwave engineers working in defense, aerospace, and precision sensing applications.

## Overview

This repository demonstrates:
- LC filter modeling using Scikit-RF's lumped element networks
- S-parameter analysis and visualization
- Monte Carlo tolerance analysis for real-world component variations
- Filter topology comparisons (L-section, T-section, π-section)
- Publication-quality figures and mathematical derivations

## Project Structure

```
lc-filter-notebooks/
├── CLAUDE.md              # Project guidelines and conventions
├── pyproject.toml         # Python project configuration (uv/pip)
├── requirements.txt       # Legacy pip requirements
├── notebooks/
│   ├── drafts/            # Work-in-progress notebooks
│   │   ├── LC_Filter_Simulation.ipynb
│   │   └── LC_Filter_Simulation_Refined.ipynb
│   └── published/         # Finalized whitepaper-ready notebooks
├── src/                   # Reusable Python modules
│   └── __init__.py
└── assets/
    ├── figures/           # Exported plots for publication
    └── data/              # Touchstone files, component models
```

## Getting Started

### Prerequisites

- Python 3.9 or higher
- [uv](https://github.com/astral-sh/uv) (recommended) or pip

### Installation

#### Option 1: Using uv (Recommended)

[uv](https://github.com/astral-sh/uv) is a fast Python package installer and resolver. Install it first:

```bash
# macOS/Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows
powershell -c "irm https://astral.sh/uv/install.ps1 | iex"

# Or with pip
pip install uv
```

Then clone and run the notebooks:

```bash
# Clone the repository
git clone https://github.com/pjdmatts/LC_Filter_ScikitRF.git
cd LC_Filter_ScikitRF

# Run Jupyter Lab (uv will automatically install dependencies)
uv run jupyter lab
```

The `uv run` command will:
- Create a virtual environment automatically
- Install all dependencies from `pyproject.toml`
- Launch Jupyter Lab with the correct Python environment

#### Option 2: Using pip with venv

```bash
# Clone the repository
git clone https://github.com/pjdmatts/LC_Filter_ScikitRF.git
cd LC_Filter_ScikitRF

# Create and activate virtual environment
python -m venv .venv
source .venv/bin/activate  # On Windows: .venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Launch Jupyter Lab
jupyter lab
```

### Running the Notebooks

1. After launching Jupyter Lab, navigate to `notebooks/drafts/`
2. Open `LC_Filter_Simulation_Refined.ipynb` for the most polished example
3. Run cells sequentially (Shift+Enter) or use "Run All" from the menu

## Key Technologies

- **[Scikit-RF](https://scikit-rf.readthedocs.io/)**: RF/microwave network analysis
- **NumPy/SciPy**: Numerical computation
- **Matplotlib**: Publication-quality visualization
- **Jupyter**: Interactive development environment

## Notebooks

### LC_Filter_Simulation_Refined.ipynb

The main whitepaper-quality notebook covering:
- **Theory**: Mathematical foundations of LC filters (impedance, resonance, transfer functions)
- **Implementation**: Building filters using Scikit-RF's Circuit class and DefinedGammaZ0 media
- **Analysis**: S-parameter plots, Smith charts, cutoff frequency calculations
- **Tolerance Analysis**: Monte Carlo simulation with ±5% inductor and ±10% capacitor tolerances
- **Topology Comparison**: L-section vs T-section filter performance

### LC_Filter_Simulation.ipynb

Educational notebook covering Scikit-RF basics:
- Connection syntax and circuit building
- DefinedGammaZ0 media class usage
- Lumped component creation
- Basic filter topologies

## Development

### Adding Reusable Functions

Place shared utility functions in `src/` for reuse across notebooks:

```python
# src/filter_utils.py
import skrf as rf

def create_lowpass_filter(freq, L_value, C_value, z0=50):
    """Create a simple LC lowpass filter."""
    media = rf.DefinedGammaZ0(frequency=freq, z0=z0)
    L = media.inductor(L_value, name='L1')
    C = media.capacitor(C_value, name='C1')
    # ... build circuit
    return circuit.network
```

Then import in notebooks:
```python
from src.filter_utils import create_lowpass_filter
```

### Exporting Figures

To save publication-quality figures:

```python
fig.savefig('../assets/figures/lowpass_sparameters.png', dpi=300, bbox_inches='tight')
```

## Contributing

This is a personal research repository. For advanced filter design services, visit the author's professional page.

## References

- [Scikit-RF Documentation](https://scikit-rf.readthedocs.io/)
- [Scikit-RF GitHub Repository](https://github.com/scikit-rf/scikit-rf)
- Pozar, D. M. (2011). *Microwave Engineering* (4th ed.). Wiley.
- Hong, J. S., & Lancaster, M. J. (2001). *Microstrip Filters for RF/Microwave Applications*. Wiley.

## License

See project repository for licensing information.

## Author

**Peter Matthews**
RF/Microwave Circuit Design
Specializing in defense, aerospace, and precision sensing applications

---

For questions or collaboration inquiries, please open an issue on GitHub.
