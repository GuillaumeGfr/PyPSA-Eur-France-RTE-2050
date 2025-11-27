# PyPSA-EUR-France-RTE-2050
This repo contains edits made to [PyPSA-Eur](https://github.com/PyPSA/PyPSA-Eur) to run a fixed capacity dispatch only model of the French grid in 2050, under scenarios from RTE's [_Futurs énergétiques 2050_](https://analysesetdonnees.rte-france.com/en/publications/energy-pathways-2050) study. These modifications were made for my BSc thesis, which ran the model to study grid congestion under the 6 scenarios presented in the study. These scenarios include data on teh genreation mix, flexibility technologies, interconnections, electricity demand, and the location of generators, which can be implemented into PyPSA-Eur with the changes shown here. The contents of this repo are not a standalone project, they should be used as an addition to the existing workflow of PyPSA-Eur

# Modelling approach
## Generation capacity
1. PV and wind: RTE gives PV and (on- and offshore) wind isntalled capacity per french region. Capacity is assigned to each region, using the existing PyPSA-Eur logic to populate generators within each region. Data is given in RTE_regions.csv




