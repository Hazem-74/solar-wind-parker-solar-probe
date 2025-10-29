# Solar Wind Parker Solar Probe

## Overview

This repository contains Jupyter notebooks for analyzing solar wind plasma and magnetic field data, primarily from the Helios 2 mission. The project focuses on understanding fundamental plasma physics, radial evolution of solar wind parameters, and the magnetic field's Parker spiral configuration.

## Project Structure

*   `SWHA_hellios2 1D.ipynb`: Analyzes 1D velocity distribution functions (VDFs) to extract plasma moments and fit various kinetic models.
*   `SWHA_hellios2 3D.ipynb`: Explores 3D solar wind and magnetic field data to study radial trends, plasma heating, and visualize the Parker spiral.
*   `Helios2_solar_wind_data.csv`: (Required for `SWHA_hellios2 3D.ipynb`) Dataset containing Helios 2 measurements from 1974 to 1980.
*   `BUE.png`, `Zewail-City.png`: (Used in notebooks) Logos for academic affiliation.

## Getting Started

### Prerequisites

Ensure you have Python 3.x installed. The notebooks primarily rely on the following libraries:

*   `numpy`
*   `pandas`
*   `scipy`
*   `matplotlib`
*   `seaborn`
*   `IPython.display` (for displaying LaTeX and GIFs within notebooks)
*   `Pillow` (for saving animations in `SWHA_hellios2 3D.ipynb`)

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Hazem-74/Solar-Wind-Parker-Solar-Probe.git
    cd Solar-Wind-Parker-Solar-Probe
    ```

2.  **Create a virtual environment (recommended):**
    ```bash
    python -m venv venv
    source venv/bin/activate  # On Windows: venv\Scripts\activate
    ```

3.  **Install the required packages:**
    ```bash
    pip install numpy pandas scipy matplotlib seaborn pillow
    ```

### Running the Notebooks

To run the Jupyter notebooks, you need to have Jupyter Notebook or JupyterLab installed.

1.  **Start Jupyter Lab/Notebook:**
    ```bash
    jupyter lab  # or jupyter notebook
    ```
    This will open a browser window displaying the repository contents.

2.  **Navigate and open:** Click on `SWHA_hellios2 1D.ipynb` or `SWHA_hellios2 3D.ipynb` to open and run the analyses.

## Notebook Summaries

### `SWHA_hellios2 1D.ipynb` - Solar Wind Heliospheric 1D Analysis

*   **Purpose:** This notebook provides a detailed 1D kinetic analysis of solar wind particle velocity distribution functions (VDFs). It focuses on extracting fundamental plasma moments and fitting various theoretical distribution models to understand the thermal and non-thermal characteristics of protons and alpha particles.

*   **Methods:**
    *   **Data Input:** Utilizes arrays of 1D velocity (`v_arr`) and corresponding distribution function (`f_arr`) values, representing real Helios 2 observations from February 9, 1976.
    *   **Extrema Detection:** Identifies local maxima and minima within the VDF using `scipy.signal.argrelextrema` to pinpoint distinct particle populations (e.g., proton core, alpha particle tail).
    *   **Distribution Fitting:** Employs `scipy.optimize.curve_fit` to fit the observed VDFs to several analytical models, providing insights into the plasma's kinetic state:
        *   **Gaussian:** A basic symmetrical model for the core population.
        *   **Maxwellian:** Describes plasma in thermal equilibrium.
        *   **Kappa:** Characterizes non-thermal, suprathermal particle populations, common in collisionless space plasmas.
        *   **Bi-Maxwellian:** Models two distinct thermal populations, potentially representing a core and a beam or halo.
        *   **Bi-Kappa:** Models two distinct non-thermal (suprathermal) populations.
        These fits are applied to the full VDF or specific velocity ranges to isolate and characterize different particle components.
    *   **Moment Calculations:** Numerically integrates the VDF using `numpy.trapz` to compute the first four fluid moments (number density, bulk velocity, variance/pressure, skewness, kurtosis) for separated proton and alpha particle populations.
    *   **Derived Quantities:** Calculates macroscopic plasma parameters such as temperature (in Kelvin and eV), thermal speed, heat flux, mean/thermal/bulk kinetic energies, entropy, and dynamic properties (e.g., dynamic pressure, Mach number, sound speed, enthalpy per unit mass).
    *   **Ratio Analysis:** Computes and displays the ratios of various alpha particle parameters to proton parameters, offering insights into their relative acceleration and heating processes.

*   **Main Logic:** The notebook proceeds by first visualizing the raw 1D VDF, then attempting to model its shape with increasingly complex theoretical fits (from Maxwellian to Bi-Kappa) to capture observed features like peaks and tails. Finally, it quantifies key physical properties of the identified proton and alpha populations through direct integration of the VDF, providing a comprehensive kinetic description of the plasma state.

### `SWHA_hellios2 3D.ipynb` - Solar Wind Heliospheric 3D Analysis

*   **Purpose:** This notebook explores the 3D evolution of solar wind plasma and magnetic field properties using an extensive dataset from the Helios 2 mission. Its aim is to visualize spatial variations, quantify radial trends, and illustrate the iconic Parker spiral.

*   **Methods:**
    *   **Data Loading:** Loads multi-parameter solar wind and magnetic field data from `Helios2_solar_wind_data.csv`, covering a period from 1974 to 1980.
    *   **Derived Quantities Calculation:** Computes a wide array of plasma and magnetic field parameters from the raw measurements, including:
        *   Magnetic field components and magnitudes (`B_r`, `B_tan`, `B`).
        *   Parallel and perpendicular specific entropies (`S_par`, `S_perp`).
        *   3D bulk velocity magnitude (`V_r`) and radial particle flux (`flux_r`).
        *   Total proton temperature (`Tp`).
        *   Various plasma betas (`beta_par`, `beta_perp`, `beta`).
        *   Alfvén speed (`V_A`) and Mach number (`M_A`).
        *   Thermal and magnetic pressures (`P_th`, `P_B`).
        *   Proton magnetic moment proxy (`mu_p`).
        *   Dynamic pressure (`P_dyn`), thermal speed (`v_th`), thermal Mach number (`M_th`), sound speed (`c_s`), and specific enthalpy (`h`).
    *   **Visualization:** Employs `matplotlib` for diverse plotting:
        *   **3D Scatter Plots:** Visualizes magnetic field components (`Bx`, `By`, `Bz`) and proton velocity components (`vpx`, `vpy`, `vpz`) in 3D space, often color-coded by the component values for enhanced insight.
        *   **2D Scatter Plots:** Illustrates relationships between key anisotropic parameters, such as parallel vs. perpendicular temperature/thermal speed and specific entropies.
        *   **Radial Profiles:** Plots various quantities against radial distance (`r_sun`) to observe their heliocentric evolution.
    *   **Moving Average Smoothing:** Applies a centered moving average to radial profiles to reduce noise and reveal underlying trends more clearly.
    *   **Power-Law Radial Fitting:** Performs linear regression in log-log space (`numpy.polyfit`) to fit power-laws (e.g., `n_p ∝ r^alpha`) for numerous parameters. The calculated exponents are then compared to theoretical predictions from MHD models.
    *   **Parker Spiral Simulation:** Numerically integrates the Parker spiral equations, using observed solar wind parameters, to visualize the interplanetary magnetic field lines. The spiral arms are animated to demonstrate the Sun's rotation, with segments colored by observed solar wind speed or magnetic field strength.

*   **Main Logic:** This notebook starts by enriching the raw Helios 2 dataset with a comprehensive set of derived plasma physics quantities. It then uses a range of visualization techniques to explore these parameters, focusing on their radial evolution and interdependencies. A key aspect is the quantitative fitting of power-law relationships to model how quantities scale with heliocentric distance, providing a direct comparison with theoretical solar wind models. The notebook concludes with an engaging visualization of the Parker spiral, a cornerstone concept in solar wind physics.

## Contributing

Feel free to fork the repository, open issues, or submit pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details (if applicable, otherwise remove this line).