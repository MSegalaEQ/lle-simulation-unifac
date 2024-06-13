# Liquid-Liquid Equilibria Simulation - UNIFAC Model

## Intro

Thermodynamic equilibria is a key system state in natural sciences. For instance, in industrial environments, heat exchangers operate under vapor-liquid equilibrium to maintain a constant temperature and offer other advantages. Liquid-liquid equilibrium is not less important, several unit operations are based on the selectivity of certain components for different liquid phases. An example of the application of liquid-liquid equilibrium is in ethanol dehydration. Distillation alone cannot achieve a higher ethanol concentration due to operational limitations, so solvent extraction may be applied.

Solvent extraction process consist in adding a secondary component, known as the extraction solvent, to the mixture of ethanol (the solute) and water (the main solvent). It is optimal for this extraction solvent to have a higher degree of molecular affinity to the solute than the main solvent, less affinity to the main solvent and must be at least feasible to separate the solute from it afterward. In real systems, there is a broad range of potential solvent candidates. Considering the costs associated with experimenting these substances, thermodynamic equilibrium simulation provides preliminary results to either screen out poor-performing substances and advance others to laboratory experimentation stage.

## Theory

Thermodynamic equilibrium simulation is grounded in the chemical equilibrium of species in multiple physical states. Chemical equilibrium can be interpretated similarly to other types of equilibrium, such as mechanical and thermal equilibrium. To illustrate this, imagine two blocks with the same mass, each hanging on the end of a rope, with the rope placed over a pulley. Naturally, if no other external force is applied, these blocks will remain still in the their positions - this is mechanical equilibrium. Two bodies in contact with different temperatures will reach the same temperature given sufficient time - this is thermal equilibrium. This is very simple to understand that mechanical systems have momentum as their measurement unit, just as thermal system have temperature. In the other hand, however, it may not be trivial for chemical systems.

The driving force of chemicals is widely known as chemical potential, but it has little or no practical use due to multiple undesired proprieties. For chemical systems, there is a wide variety of measurements depending on the process. All these measurements are linked to Gibbs free energy. For liquid phases, activity coefficient and Gibbs free energy in excess are the metrics involved and the equilibrium of three species is achieved when:
$$\gamma_A^{(1)} x_A^{(1)} = \gamma_A^{(2)} x_A^{(2)}$$
$$\gamma_B^{(1)} x_B^{(1)} = \gamma_B^{(2)} x_B^{(2)}$$
$$\gamma_C^{(1)} x_C^{(1)} = \gamma_C^{(2)} x_C^{(2)}$$

As:
- $x_i^{(1)}$: mole fraction of species $i$ in Phase 1.
- $x_i^{(2)}$: mole fraction of species $i$ in Phase 2.
- $\gamma_i^{(1)}$: activity coefficient of species $i$ in Phase 1.
- $\gamma_i^{(2)}$: activity coefficient of species $i$ in Phase 2.

The role of molecular models in these calculations is to integrate molecular particularities quantitatively, that is, to estimate activity coefficients. The UNIFAC model consists of the sum of two terms: one carrying molecular properties according to a list of interaction parameters and the other carrying probabilistic uncertainty. The complete model equations and details can be found [here](https://en.wikipedia.org/wiki/UNIFAC), more instructions over molecular subgroup assigments [here](http://www.aim.env.uea.ac.uk/aim/info/UNIFACgroups.html) and interaction parameters values [here](https://www.ddbst.com/published-parameters-unifac.html).

## The Algorithm

The system of equations has 12 variables and 3 equations, therefore cannot be solved immediately. First, there are two additional equations not mentioned before, one for each phase: the sum of species' molar fractions (composition on a molar basis) must be equal 1. Molelucar model will estimate all activity coefficients, however, it can only estimate these values once the molar fractions are known, and these are also unknown variables. With 5 equations and 6 implicit variables, an iterative process is required to solve this problem:

1. Start by guessing any of the solvent molar fractions in one exclusive phase, preferably stting it to 0.99, so that the others will be near zero. For instance, $x_A^{(1)}$ = 0.99;
2. Calculate activity coefficients;
3. Solve the system of equations for all the molar fractions but the one guessed and store results;
4. Increment the other solvent by a given quantity $s$ and repeat these steps until $x_B^{(1)}$ = 0.99.

By calculating activity coefficients before solving the system of equations, step 3 can be carried out by calculating the inverted matrix of linear coefficients, as [it is a pseudo-linear system of equations](https://www.sciencedirect.com/science/article/pii/S0098135412003729).

## Excel Spreadsheet

This repository presents an [Excel spreadsheet](UNIFAC LLE.xlsm) containing all the necessary resources to simulate any ternary dual liquid phase system. Visual Basic was used for the implementation of the calculation algorithm, along with scripted buttons. Instructions are provided in the first sheet.

Some observations: mixtures that do not separate into multiple phases may return errors during calculations. For some systems, different starting concentrations may be required, and reordering species may solve this problem without editing the programmed script. As a prototype version, this spreadsheet may produce other types of error not yet debugged.
