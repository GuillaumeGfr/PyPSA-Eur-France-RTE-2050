# PyPSA-EUR-France-RTE-2050
This repo contains edits made to [PyPSA-Eur](https://github.com/PyPSA/PyPSA-Eur) to run a fixed capacity dispatch only model of the French grid in 2050, under scenarios from RTE's [_Futurs énergétiques 2050_](https://analysesetdonnees.rte-france.com/en/publications/energy-pathways-2050) study. These modifications were made for my BSc thesis, which ran the model to study grid congestion under the 6 scenarios presented in the study. These scenarios include data on teh genreation mix, flexibility technologies, interconnections, electricity demand, and the location of generators, which can be implemented into PyPSA-Eur with the changes shown here. The contents of this repo are not a standalone project, they should be used as an addition to the existing workflow of PyPSA-Eur

# Modelling approach
## Generation capacity
1. PV and wind: RTE gives PV and (on- and offshore) wind installed capacity per French region. Capacity is assigned to each region, using the existing PyPSA-Eur logic to populate wind and PV generators within the regions. This data is given in [RTE_regions.csv](Custom%20data/RTE_regions.csv).
2. Hydro: There is no available data on the location of new hydro plants, so the additional capacity in 2050 was distributed over all existing facilities. PHS was added as a store with fixed capacity of 8GW in all scenarios.
3. Nuclear: In 'M' scenarios, reactors opened after 1990 are kept, for a total installed capacity of 15.5GW of existing nuclear. In 'N' scenarios, the newly built reactors are distributed across existing sites, in line with current plans. N03 includes extending the lifetime of some reactors by 3-5 additional years, raising existing nuclear fleet to 24GW. SMRs were modeled as 500MW units only located on existing nuclear sites.

Nuclear and Hydro plants are populated on the network using the same logic as standard PyPSA-Eur, only the source was modified: it now picks from a scenario-specific csv file instead of the powerplantmatching database.

## Flexibility
1. Batteries: Modelled as a storage unit, all parameters except for installed capacity left unchanged from the original PyPSA-Eur logic. The location of batteries on the network is determined by load.
2. Power-to-gas-to-power: This model avoids sector coupling by modelling P2G2P as links and stores, with p2G links representing electrolysers, stores representing Hydrogen storage, and G2P links representing OCGT or CCGT powerplants. Characteristics of each are given in [flexibility.csv](Custom%20data/flexibility.csv). The location of P2G and G2P links on the network is determined by installed renewables capacity, and demand: P2G links are located near renewable generators, and G2P links near loads.
3. Interconnections: Since the analysis focuses on France, neighbouring countries are represented as one aggregated bus each, with fixed hourly imports/exports from ENTSO-E’s TYNDP 2024 [model outputs](https://2024.entsos-tyndp-scenarios.eu/download/). This data is shown in [cross_border_flows.csv](Custom%20data/cross_border_flows.csv). 
   * No internal generation or demand is modelled in neighbouring countries.
   * Weather years are not aligned across countries.
   * Cross-border flows are scaled to match interconnection capacities.
   * This block is included for reference; it is not required for implementing RTE 2050 scenarios.
  
## Load
The load curve was taken from the year 2023 (because it is the most recent year with readily available weather datasets for PyPSA-Eur), and scaled up to reach the total yearly load of 645TWh in RTE's baseline scenario. The timestamps for demand are therefore 2023, because PyPSA-Eur's original pipeline to fetch weather data was kept unchanged.
  
## Limitations
This setup was created for a specific goal: to model congestion in the French grid under generation mixes from RTE's 2050 scenarios. As such, it has inherent limitations:
* The predetermined cross-border flows were made under weather year 2009, while the rest of the model runs with weather data from 2023. The logic to add the interconnections to the network is usable, however their behaviour is not aligned with the rest of the model.






