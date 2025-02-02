# What are immiscible liquids? Liquid-Liquid Equilibria Simulation with UNIFAC Model

## Intro

An immiscible (or non-miscible) liquid mixture is when two liquids do not mix together but instead stay separate, like oil and water. When oil is poured into a glass of water, it floats on top instead of blending in, resulting in two layers of liquids, often called distinct liquid phases. Some everyday examples of immiscible liquids include salad dressing (oil and vinegar) or gasoline and water.

However, no two liquids are completely immiscible. In reality, each phase of the mixture contains a tiny amount of the other liquid phase component. For example, even though oil and water form separate phases, a very small amount of water dissolves into the oil, and a tiny bit of oil dissolves into the water. This happens because molecules interact at a microscopic level, even if they don’t mix visibly.

The reason behind this behavior lies mainly in the intermolecular forces and polarity of the molecules. Intermolecular forces are the forces between molecules that determine whether they will mix. Water forms strong bonds between its molecules, which oil cannot easily break, leading to phase separation. Polarity refers to how the molecules are charged. Water molecules have positive and negative poles at their ends that attract other water molecules, while oil molecules are neutral and are less affected by electric charges.

Thermodynamics provides a framework to quantify how molecules interact with each other and how those interactions affect the overall behavior of a mixture, specifically the system equilibrium composition. One way to estimate phase compositions is to use predictive models like UNIFAC (Universal Quasi-Chemical Functional Group Activity Coefficients). These models "translate" molecular properties into quantitative parameters that attempt to reproduce their behavior in the real world. This allows us to calculate how different molecular groups interact, predicting the concentration of each substance in both phases. Such estimations are essential for applications in chemical engineering, where understanding phase behavior is crucial for designing separation processes like distillation or liquid-liquid extraction.

This text presents a simplified theory of liquid-liquid equilibrium along with a methodology for a ternary system and includes an Excel spreadsheet with the algorithm implemented.

## Theory

Thermodynamic equilibrium simulation is grounded in the chemical equilibrium of species in multiple physical states. Chemical equilibrium can be interpretated similarly to other types of equilibrium, such as mechanical and thermal equilibrium. To illustrate this, imagine two blocks with the same mass, each hanging on the end of a rope, with the rope placed over a pulley. Naturally, if no other external force is applied, these blocks will remain still in the their positions - this is mechanical equilibrium. Two bodies in contact with different temperatures will reach the same temperature given sufficient time - this is thermal equilibrium. This is very simple to understand that mechanical systems have momentum as their measurement unit, just as thermal system have temperature. In the other hand, however, it may not be trivial for chemical systems.

The driving force of chemicals is widely known as chemical potential, but it has little or no practical use due to multiple undesired proprieties. For chemical systems, there is a wide variety of measurements depending on the process. All these measurements are linked to Gibbs free energy. For liquid phases, activity coefficient and Gibbs free energy in excess are the metrics involved and the equilibrium of, for instance, three species is achieved when:
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

This repository presents an [Excel spreadsheet](https://github.com/MSegalaEQ/lle-simulation-unifac/raw/main/UNIFAC%20LLE.xlsm) containing all the necessary resources to simulate any ternary dual liquid phase system. Visual Basic was used for the implementation of the calculation algorithm, along with scripted buttons. Instructions are provided in the first sheet.

Some observations: mixtures that do not separate into multiple phases may return errors during calculations. For some systems, different starting concentrations may be required, and reordering species may solve this problem without editing the programmed script. As a prototype version, this spreadsheet may produce other types of error not yet debugged.
