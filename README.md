# Optimized Features of Hybrid Vegetation-Seawall Coastal Systems

## Description

This repository contains the dataset and MATLAB code used for the paper "Optimized Features of Hybrid Vegetation-Seawall Coastal Systems: Analytics for Design." This project aims to analyze and optimize the design of coastal systems that incorporate both vegetation and seawall structures to enhance flood protection and serviceability.

The MATLAB scripts in this repository include functions for calculating key metrics such as wave runup, overtopping, robustness, and serviceability of coastal structures. The dataset includes various input parameters and results from simulations conducted for the study.

## Repository Structure

```
Your-Repository-Name/
├── data/
│   └── [Your dataset files]
├── code/
│   ├── AAA_Run_all.m
│   ├── AFF_making_input_files.m
│   ├── AFF_nonlinear_linspace_decrease.m
│   ├── FFF_Bed_creation.m
│   ├── FFF_Change_txt_files.m
│   ├── FFF_Modify_stations_locations.m
│   ├── FFF_Overtopping.m
│   ├── FFF_Robustness.m
│   ├── FFF_Runup.m
│   ├── FFF_Serviceability.m
│   ├── FFF_Steepness.m
│   └── FFF_Vegrigidmap.m
├── README.md
└── [Other files]
```

## Installation

1. **Clone the repository:**
   ```sh
   git clone https://github.com/your-username/your-repository-name.git
   ```

2. **Navigate to the repository directory:**
   ```sh
   cd your-repository-name
   ```

3. **Ensure you have MATLAB installed on your machine to run the scripts.**

## Usage

1. **Place your dataset files in the `data/` directory.**

2. **Run the MATLAB scripts located in the `code/` directory to perform the analysis:**
   - `AAA_Run_all.m`: Main script to run all analyses.
   - `AFF_making_input_files.m`: Script to create input files.
   - `AFF_nonlinear_linspace_decrease.m`: Script for nonlinear linspace decrease.
   - `FFF_Bed_creation.m`: Function to create bed profiles.
   - `FFF_Change_txt_files.m`: Function to change text files.
   - `FFF_Modify_stations_locations.m`: Function to modify station locations.
   - `FFF_Overtopping.m`: Function to calculate wave overtopping.
   - `FFF_Robustness.m`: Function to calculate robustness and dry height.
   - `FFF_Runup.m`: Function to calculate wave runup.
   - `FFF_Serviceability.m`: Function to calculate serviceability.
   - `FFF_Steepness.m`: Function to calculate wave steepness.
   - `FFF_Vegrigidmap.m`: Function to create a vegetation rigid map.

3. **Example:**
   ```matlab
   % Example to run the main script
   run('code/AAA_Run_all.m');
   ```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request or open an Issue if you have any suggestions or find any bugs.

## License
This code is under Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0) License. More information can be find in the license file. 

This project is funded, in part, by the US Coastal Research Program (USCRP) as administered by the US Army Corps of Engineers® (USACE), Department of Defense (Contract No. W912HZ22C0010). The content of the information provided in this publication does not necessarily reflect the position or the policy of the government, and no official endorsement should be inferred. The authors acknowledge the USACE and USCRP’s support of their effort to strengthen coastal academic programs and address coastal community needs in the United States.

## Contact

For any questions or further information, please contact Erfan Amini at [eamini@stevens.edu].
