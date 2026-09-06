<div align="center">
  <img src="./assets/banner.svg" alt="Valliente Aerospace Systems Engineering Laboratory" width="100%" />
</div>

<div align="center">
  <img src="https://img.shields.io/badge/Discipline-Aerospace%20Engineering-f59e0b?style=flat-square&labelColor=0d1117" alt="Discipline" />
  <img src="https://img.shields.io/badge/Specialization-Propulsion%20%26%20Flight%20Dynamics-f59e0b?style=flat-square&labelColor=0d1117" alt="Specialization" />
  <img src="https://img.shields.io/badge/Astrodynamics-Trajectory%20%26%20Orbit%20Mechanics-f59e0b?style=flat-square&labelColor=0d1117" alt="Astrodynamics" />
  <img src="https://img.shields.io/badge/Physics%20Engine-6--DoF%20RK4%20%2B%20CR3BP-f59e0b?style=flat-square&labelColor=0d1117" alt="Physics Engine" />
  <a href="https://github.com/valliente?tab=repositories&sort=stargazers">
    <img src="https://img.shields.io/github/stars/valliente?style=flat-square&color=f59e0b&labelColor=0d1117&logo=github&logoColor=f59e0b" alt="Total Stars" />
  </a>
</div>

<br/>

Aerospace engineer specializing in numerical flight trajectory integration, supersonic propulsion, compressible gas dynamics, and orbital mechanics. Developing computational simulation engines, flight dynamics solvers, and real-time telemetry workstations across atmospheric ascent and orbital transfer regimes.

---

### Flight Telemetry & Activity Metrics

<div align="center">
  <img src="https://github-readme-stats-fast.vercel.app/api?username=valliente&show_icons=true&hide_rank=true&include_all_commits=true&count_private=true&bg_color=0d1117&title_color=f59e0b&text_color=f8fafc&icon_color=f59e0b&border_color=262f3d" height="165" alt="Engineering Telemetry" />
  <img src="https://github-readme-stats-fast.vercel.app/api/top-langs/?username=valliente&layout=compact&bg_color=0d1117&title_color=f59e0b&text_color=f8fafc&border_color=262f3d&langs_count=8" height="165" alt="Language Distribution" />
</div>

<div align="center">
  <img src="https://streak-stats.demolab.com/?user=valliente&background=0D1117&border=262F3D&stroke=F59E0B&ring=F59E0B&fire=F59E0B&currStreakNum=FFFFFF&sideNums=FFFFFF&currStreakLabel=F59E0B&sideLabels=CBD5E1&dates=94A3B8" height="165" alt="Contribution Streak" />
</div>

---

### Flight Regimes & Trajectory Integrator Specifications

| Flight Regime | Operational Domain | Physical Mechanisms & Governing Laws | Simulation Architecture & Solvers |
| :--- | :--- | :--- | :--- |
| **Atmospheric Ascent** | $0 \le M < 1.2$<br>$h: 0 \to 12\text{ km}$ | Max-Q dynamic pressure ($q = \frac{1}{2}\rho v^2$), 1976 US Standard Atmosphere piecewise barometric layering, Barrowman static stability margin ($\text{SM} = \frac{x_{\text{CP}} - x_{\text{CM}}}{D_{\text{ref}}}$), and gravity turn pitchover dynamics. | 2-DoF / 6-DoF Runge-Kutta 4th Order (RK4) with adaptive step size control, quaternion kinematics, and mass property depletion tracking. |
| **Supersonic & Hypersonic** | $1.2 \le M < 25$<br>$h: 12 \to 85\text{ km}$ | Oblique shock wave detachment, Modified Newtonian impact theory ($C_p = C_{p,\text{max}} \sin^2\theta$), aerodynamic heating, high-temperature shock dissociation, and RF plasma ionization blackout attenuation ($n_e(v, \rho)$). | SIMD-vectorized hypersonic surface panel pressure integrators, real-time RF link budget attenuation curves across S, X, and Ka bands. |
| **Orbital Injection** | $M \ge 25$<br>$h: 100 \to 500\text{ km}$ | Vacuum specific impulse ($I_{\text{sp,vac}}$), multi-stage Tsiolkovsky staging, Closed-Loop Powered Explicit Guidance (PEG: $\hat{\mathbf{u}}(t) = \mathbf{A} + \mathbf{B}(t - t_0)$), and Keplerian two-body orbit propagation. | Analytical PEG steering converging to target orbital radius $r_T$ and cutoff velocity $v_T$; orbital element state vectors $(\mathbf{a}, e, i, \Omega, \omega, \nu)$. |
| **Cislunar Astrodynamics** | $r > 50{,}000\text{ km}$<br>Lagrange Points | Circular Restricted Three-Body Problem (CR3BP) rotating barycentric frame, Jacobi energy integral conservation ($C = 2\Omega - v^2$), Halo orbits, and Earth-Moon invariant manifolds. | Symplectic numerical integrators, collinear ($L_1, L_2, L_3$) and triangular ($L_4, L_5$) Lagrange point equilibrium solvers, Apollo Free-Return trajectories. |

---

### Mathematical Physics Dossiers

<details>
<summary><b>1. 6-DoF Rigid-Body Dynamics & Quaternion Kinematics</b></summary>
<br/>

The translational and rotational states of a flight vehicle are propagated via a 14-element state vector $\mathbf{x} = [\mathbf{r}, \mathbf{v}, \mathbf{q}, \boldsymbol{\omega}, m]^T$:

$$\dot{\mathbf{x}} = \begin{bmatrix} \dot{\mathbf{r}} \\ \dot{\mathbf{v}} \\ \dot{\mathbf{q}} \\ \dot{\boldsymbol{\omega}} \\ \dot{m} \end{bmatrix} = \begin{bmatrix} \mathbf{v} \\ \frac{1}{m}\left(\mathbf{T}_I + \mathbf{D}_I + \mathbf{L}_I\right) + \mathbf{g}(\mathbf{r}) \\ \frac{1}{2} \mathbf{\Omega}(\boldsymbol{\omega}) \mathbf{q} \\ \mathbf{I}^{-1}\left(\mathbf{M}_B - \boldsymbol{\omega} \times (\mathbf{I}\boldsymbol{\omega})\right) \\ -\frac{\|\mathbf{T}\|}{I_{\text{sp}} g_0} \end{bmatrix}$$

where the skew-symmetric quaternion rate matrix avoids gimbal lock singularities:

$$\mathbf{\Omega}(\boldsymbol{\omega}) = \begin{bmatrix} 0 & -\omega_x & -\omega_y & -\omega_z \\ \omega_x & 0 & \omega_z & -\omega_y \\ \omega_y & -\omega_z & 0 & \omega_x \\ \omega_z & \omega_y & -\omega_x & 0 \end{bmatrix}$$

Dynamic mass depletion directly updates the instantaneous inertia tensor $\mathbf{I}(t) = \text{diag}(I_{xx}, I_{yy}, I_{zz})$ and center of mass $x_{\text{CM}}(t)$.
</details>

<details>
<summary><b>2. Supersonic de Laval Nozzle Thermodynamics & Compressible Flow</b></summary>
<br/>

Compressible gas dynamics through converging-diverging de Laval nozzles govern propulsion efficiency. The cross-sectional area ratio to throat area ($A / A^*$) is related to Mach number $M$ and specific heat ratio $\gamma$:

$$\frac{A}{A^*} = \frac{1}{M}\left[\frac{2}{\gamma+1}\left(1 + \frac{\gamma-1}{2}M^2\right)\right]^{\frac{\gamma+1}{2(\gamma-1)}}$$

Stagnation-to-static thermodynamic ratios throughout the expansion:

$$\frac{T_0}{T} = 1 + \frac{\gamma-1}{2}M^2, \quad \frac{p_0}{p} = \left(1 + \frac{\gamma-1}{2}M^2\right)^{\frac{\gamma}{\gamma-1}}$$

Total effective vacuum thrust accounts for exit momentum and pressure differential across exit plane area $A_e$:

$$F = \dot{m} v_e + (p_e - p_a) A_e = C_F \cdot p_c \cdot A_t$$

where $C_F$ is the thrust coefficient and $p_a$ is local ambient barometric pressure evaluated via the 1976 US Standard Atmosphere.
</details>

---

### Featured Aerospace & Engineering Systems

| Project | Description | Performance & Architecture |
| :--- | :--- | :--- |
| [**AeroPro: Rocketry & Propulsion Studio**](https://github.com/valliente/aeropro-rocketry-studio) | Complete aerospace rocketry suite with 1976 US Standard Atmosphere, multi-stage Tsiolkovsky solver, 2-DoF RK4 flight integrator, and isentropic nozzle visualizer. | `>500,000 steps/sec`<br>Tauri v2 • Rust WASM SIMD • React 19 • Three.js |
| [**CRISPR Target Designer**](https://github.com/valliente/crispr-target-designer) | Standalone desktop bioinformatics workbench for multi-nuclease gRNA design, double-strand break mapping, Hsu-Zhang mismatch scoring, and Golden Gate cloning oligo synthesis. | Native Standalone GUI<br>Python 3.11 • PyQt6 • Biopython • NumPy |
| [**LagForge**](https://github.com/valliente/lagforge) | Windows network conditioner and latency simulator featuring kernel-level WinDivert packet filtering, Gaussian jitter, packet loss injection, and zero-drop teardown. | Zero-Drop Packet Filter<br>Python • PySide6 • WinDivert • Win32 API |
| [**Microbot Evolution Lab**](https://github.com/valliente/microbot-evolution-lab) | Autonomous artificial life and genetic evolution simulation featuring rule-based vector steering, asexual reproduction with trait mutation, and O(1) spatial hash grids. | 60 FPS Continuous Grid<br>TypeScript • React 18 • HTML5 Canvas • WebGL |
| [**Ghost Runner**](https://github.com/valliente/ghost-runner) | Hybrid 2D side-scrolling fitness engine featuring real-time GPS Kalman filtering, WebRTC P2P ghost racing, and multi-track Tone.js audio synthesis. | Real-Time Telemetry P2P<br>Phaser 3 • Tone.js • Tauri v2 • Capacitor |
| [**Horology Studio 3D**](https://github.com/valliente/horology-studio-3d) | Native desktop customizer for 3D luxury mechanical watches with dynamic dial texture mapping and PBR sapphire and steel shaders. | PBR Shader Pipeline<br>C++20 • Qt 6 Quick 3D • QML • CMake |
| [**Micro Timegrapher**](https://github.com/valliente/micro-timegrapher) | Acoustic mechanical watch diagnostic workstation with WASM DSP autocorrelation, acoustic telemetry capture, and positional rate analysis. | Acoustic DSP Engine<br>WASM • WebAudio DSP • React • TypeScript |
| [**Cellular Pathway Simulator**](https://github.com/valliente/cellular-pathway-simulator) | Interactive biochemistry metabolic pathway simulator with enzymatic reaction kinetics, node-graph pathway visualization, and flux analytics. | Kinetic Flux Engine<br>React • React Flow • TypeScript • Zustand |

---

### Computational Toolchain & Engineering Stack

<div align="center">
  <img src="https://skillicons.dev/icons?i=cpp,c,rust,python,ts,wasm,qt,react,tailwind,linux,docker,git&theme=dark" alt="Computational Toolchain" />
</div>

<br/>

| Domain | Frameworks, Engines & Languages | Key Capabilities |
| :--- | :--- | :--- |
| **Flight Dynamics & Numerical Integration** | `C++20` `Rust` `Python` `NumPy` `SciPy` `WASM SIMD` | 2-DoF / 6-DoF RK4/RKF45 integrators, quaternion kinematics, 1976 US Standard Atmosphere, aerodynamic drag polars ($C_D(\text{Mach})$). |
| **Rocket Propulsion & Gas Dynamics** | `Rust` `C++20` `TypeScript` `Tauri v2` | Multi-stage Tsiolkovsky delta-v solver, isentropic converging-diverging nozzle expansion, shock diamond visualizer. |
| **Astrodynamics & Orbit Propagation** | `Python` `C++20` `WebGL` `Three.js` | Keplerian state propagation, circular restricted three-body problem (CR3BP), Powered Explicit Guidance (PEG). |
| **Avionics UI & Telemetry Visualization** | `Qt 6 (Quick 3D)` `QML` `Tauri v2` `React 19` `Phaser 3` | Hardware-accelerated 60 FPS flight gauges, PBR shaders, real-time GPS Kalman filtering, WebAudio DSP autocorrelation. |
| **Systems & Platform Engineering** | `CMake` `MSVC` `Win32 API` `WinDivert` `Linux` `Git` | Native standalone zero-dependency compilation, kernel-level packet filtering, automated CI/CD multi-architecture release pipelines. |
