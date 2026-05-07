[README.md](https://github.com/user-attachments/files/27487957/README.md)
# Thin-Film Optical Coating Simulator

Thin-Film Optical Coating Simulator is an interactive browser tool for testing multilayer optical coating designs. It generates possible layer sequences from user-defined materials, evaluates their reflectance and transmittance over a wavelength range, ranks the results, and visualizes the best coating candidates.

The simulator is designed for quick exploration of quarter-wave thin-film stacks. It can be used to compare reflector-oriented and transmitter-oriented coating arrangements without installing any software or running a backend server.

## What The Simulator Does

The app models stacks made from optical materials with user-defined refractive indices. Each material can be assigned a layer count, and the simulator generates the distinct coating permutations that can be built from those layers.

For each coating sequence, the app computes:

- Reflectance curve, `R(lambda)`
- Transmittance curve, `T(lambda)`
- Reflectance at the design wavelength
- Transmittance at the design wavelength
- Average reflectance/transmittance across the wavelength sweep
- Minimum and maximum reflectance values across the sweep
- Approximate reflectance bandwidth

Results are displayed through interactive plots and a sortable rankings table.

## Main Features

### Editable Physical Setup

The left panel lets the user define the optical setup:

- Design wavelength, `lambda0`
- Wavelength sweep minimum and maximum
- Incident medium refractive index
- Substrate refractive index

These values control the wavelength range and boundary conditions used in the thin-film simulation.

### Custom Material System

The simulator starts with two default materials:

- `H`: high-index material
- `L`: low-index material

Each material has:

- Refractive index, `n`
- Layer count

Additional materials can be added with the `Add Material` button. New materials are named alphabetically, such as `A`, `B`, and `C`.

### Automatic Coating Generation

The app generates all unique coating sequences that can be formed from the selected material counts. For example, if `H` has count `4` and `L` has count `4`, the simulator evaluates the unique permutations of those eight layers.

To keep the browser responsive, the app limits extremely large permutation sets and asks the user to reduce material counts when the combination count is too high.

### Best Reflector Ranking

The best reflector is selected using a layered ranking rule:

1. Highest reflectance at the design wavelength, `R_eval`
2. If tied, highest average reflectance over the sweep, `R_avg`
3. If still tied, highest minimum reflectance over the sweep, `R_min`
4. If still tied, alphabetical coating order

This helps prefer coatings that are not only strong at the design wavelength, but also more stable over the wavelength range.

### Best Transmitter Ranking

The best transmitter is selected independently from the reflector ranking:

1. Highest transmittance at the design wavelength, `T_eval`
2. If tied, highest average transmittance over the sweep, `T_avg`
3. If still tied, highest minimum transmittance over the sweep, `T_min`
4. If still tied, alphabetical coating order

This avoids choosing a transmitter only because it is the weakest reflector at one wavelength. The selected transmitter is the strongest overall candidate according to the transmittance metrics.

### Interactive Plot Tabs

The right panel contains several views:

#### All Coatings

Shows all simulated coating reflectance curves. The strongest reflector and strongest transmitter are highlighted for comparison.

#### Best R & T

Shows the best reflector and best transmitter side by side. Reflectance and transmittance curves can be toggled independently using the plot legend.

#### Top-N Reflectors

Displays the top reflector candidates. The user can choose how many coatings to show and optionally display transmittance curves alongside reflectance curves.

#### Rankings Table

Displays coating results in a sortable table. The `R @ design lambda` and `T @ design lambda` columns use the same ranking rules as the best reflector and best transmitter selection.

#### Inspector

Allows the user to type a custom coating sequence, such as:

```text
H L H L H L H L
```

The Inspector plots `R(lambda)` and `T(lambda)` for that exact sequence and reports the values at the design wavelength.

### CSV Export

The `Export CSV` button downloads the simulation results as a CSV file. The export includes each coating, its design-wavelength reflectance/transmittance, and the reflectance values across the wavelength sweep.

### Theme Support

The interface supports dark and light themes. Plots, tables, controls, and the Inspector view update with the selected theme.

## How To Use

1. Set the design wavelength.
2. Set the wavelength sweep range.
3. Enter the incident and substrate refractive indices.
4. Adjust the refractive index and count for each material.
5. Add extra materials if needed.
6. Check the possible coating count.
7. Click `Run Simulation`.
8. Review the best reflector and best transmitter values in the left panel.
9. Use the plot tabs to compare coatings visually.
10. Open the rankings table to sort and inspect candidates.
11. Use the Inspector tab to test a specific coating sequence.
12. Export the results as CSV if further analysis is needed.

## Technical Notes

- The app runs entirely in the browser.
- No backend server is required.
- Plotting is handled with Plotly.
- The optical calculation uses a transfer matrix style method for normal-incidence thin-film stacks.
- Transmittance is treated as `T = 1 - R`, assuming no absorption losses.
- Layer thicknesses are modeled as quarter-wave optical thicknesses at the design wavelength.

## Limitations

- The model assumes non-absorbing materials.
- Only normal-incidence behavior is represented.
- Dispersion is not included; each material uses a constant refractive index.
- Very large material counts can create too many unique permutations for real-time browser simulation.

## Suggested Use Cases

- Exploring high-reflectance stack arrangements
- Comparing candidate transmitter coatings
- Teaching or demonstrating thin-film interference behavior
- Quickly screening quarter-wave coating sequences before deeper optical modeling

