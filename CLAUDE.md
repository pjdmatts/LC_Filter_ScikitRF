# LC Filter Simulation Notebooks

## Project Purpose

This repository contains Jupyter notebooks that simulate LC (inductor-capacitor) filter behavior using **Scikit-RF**. The notebooks are designed to become polished technical whitepapers suitable for publication on platforms like Gumroad, targeting engineers and technical professionals working in RF/microwave design, particularly in defense, aerospace, and precision sensing applications.

## Folder Structure

```
lc-filter-notebooks/
├── CLAUDE.md              # This file
├── notebooks/
│   ├── drafts/            # Work-in-progress notebooks
│   └── published/         # Finalized whitepaper-ready notebooks
├── src/                   # Reusable Python modules
│   └── __init__.py
└── assets/
    ├── figures/           # Exported plots for publication
    └── data/              # Touchstone files, component models, etc.
```

## Technology Stack

- **Scikit-RF** (`skrf`): Primary library for network analysis and S-parameter manipulation
- **NumPy/SciPy**: Numerical computation
- **Matplotlib**: Publication-quality figures
- **Jupyter**: Notebook environment

## Scikit-RF Conventions

### Core Objects
```python
import skrf as rf

# Frequency object (always define explicitly)
freq = rf.Frequency(start=1, stop=1000, npoints=1001, unit='MHz')

# Network from S-parameters or circuit elements
ntwk = rf.Network(...)

# Media for transmission line and lumped elements
from skrf.media import DefinedGammaZ0
media = DefinedGammaZ0(frequency=freq, z0=50)
```

### LC Component Creation
```python
# Lumped elements via media
L = media.inductor(L=10e-9)    # 10 nH inductor
C = media.capacitor(C=100e-12) # 100 pF capacitor
R = media.resistor(R=50)       # 50 ohm resistor

# Cascade with **
filter_network = L ** C ** L
```

### Circuit Class (Preferred for Complex Filters)
```python
from skrf import Circuit

# Define circuit with port connections
circuit = Circuit([
    [(port1, 0), (inductor, 0)],
    [(inductor, 1), (capacitor, 0), (port2, 0)],
    [(capacitor, 1), (gnd, 0)]
])
filter_ntwk = circuit.network
```

### S-Parameter Access
```python
ntwk.s[:, 0, 0]  # S11 (reflection) as array
ntwk.s[:, 1, 0]  # S21 (transmission) as array
ntwk.s_db        # S-parameters in dB
ntwk.z           # Z-parameters
ntwk.y           # Y-parameters
```

## Notebook Standards for Whitepaper Production

### Structure
1. **Title cell**: Clear, descriptive title as H1 markdown
2. **Abstract**: 2-3 sentence summary of what the notebook demonstrates
3. **Theory section**: Mathematical foundation with LaTeX equations
4. **Implementation**: Code cells with thorough comments
5. **Results**: Visualizations with interpretation
6. **Conclusions**: Key takeaways and practical implications
7. **References**: Cited sources

### Code Style
- Explicit imports at notebook top
- Define `freq` object early and reuse throughout
- Use descriptive variable names: `lowpass_filter` not `lpf`
- Include units in comments: `# L1 = 47 nH`
- Wrap reusable logic in functions with docstrings

### Figure Standards
```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(figsize=(8, 5))
ntwk.plot_s_db(m=1, n=0, ax=ax, label='S21')
ax.set_xlabel('Frequency (MHz)')
ax.set_ylabel('Magnitude (dB)')
ax.set_title('Lowpass Filter Response')
ax.legend()
ax.grid(True, alpha=0.3)
plt.tight_layout()
# For publication: fig.savefig('../assets/figures/filename.png', dpi=300, bbox_inches='tight')
```

### LaTeX in Markdown Cells
Use for transfer functions, impedance equations, etc.:
```markdown
The impedance of an inductor: $Z_L = j\omega L$

The resonant frequency: $$f_0 = \frac{1}{2\pi\sqrt{LC}}$$
```

## Common Filter Topologies to Cover

- **Lowpass**: Series-L, shunt-C ladder
- **Highpass**: Series-C, shunt-L ladder  
- **Bandpass**: Series-LC, shunt-LC resonators
- **Bandstop/Notch**: Parallel LC in series path
- **Butterworth/Chebyshev**: Standard filter synthesis

## Quality Checklist Before Publishing

- [ ] All cells execute without error (Kernel → Restart & Run All)
- [ ] Equations render correctly
- [ ] Figures have proper labels, legends, and readable fonts
- [ ] Code is commented explaining the "why" not just the "what"
- [ ] Theory section provides sufficient background for target audience
- [ ] Conclusions offer actionable insights
- [ ] No hardcoded paths (use relative paths)
- [ ] Notebook cleared of stale outputs before commit

## Helper Module Pattern

Place reusable functions in `src/`:

```python
# src/filter_utils.py
import skrf as rf
from skrf.media import DefinedGammaZ0

def create_media(freq, z0=50):
    """Create a DefinedGammaZ0 media for lumped element synthesis."""
    return DefinedGammaZ0(frequency=freq, z0=z0)

def butterworth_lowpass(freq, fc, order, z0=50):
    """
    Synthesize a Butterworth lowpass filter.
    
    Parameters
    ----------
    freq : rf.Frequency
        Frequency sweep object
    fc : float
        Cutoff frequency in Hz
    order : int
        Filter order
    z0 : float
        Characteristic impedance
        
    Returns
    -------
    rf.Network
        Filter network
    """
    # Implementation using standard g-values
    ...
```

## Useful Scikit-RF Resources

- Documentation: https://scikit-rf.readthedocs.io/
- GitHub: https://github.com/scikit-rf/scikit-rf
- Examples: https://scikit-rf.readthedocs.io/en/latest/examples/index.html
- Circuit tutorial: https://scikit-rf.readthedocs.io/en/latest/tutorials/Circuit.html

## Notes

- Target audience: RF engineers with basic filter theory knowledge
- Tone: Technical but accessible, bridging theory and practical application
- Length: Aim for notebooks that take 10-15 minutes to read through
