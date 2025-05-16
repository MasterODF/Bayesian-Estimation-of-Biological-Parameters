# Bayesian-Estimation-of-Biological-Parameters
 ------------------- WORK IN PROGRESS ---------------------------
Code for estimation of mutation rates for cell cultures which are subjected to a change in medium: from a safe one to another where only certain mutations of the original cells are able to survive. More info (hopefully!) in a following paper.

For now, due to how it's programmed, the project is only useful for simulations where your data files are formatted to include the number of mutated cells in each time step. However, the methods used don't need this info, and it's only a matter of time until we can get rid of that (really, unnecessary) info. It's only there because of the way the code uses the format.


- What it does?
This project was part of an extracurricular project from my Physics degree. Its main objective is to estimate the mutation rate of a population of cultures by combining bayesian estimators and the Luria-Delbruck distribution. There is also available a quick implementation of the Gillespie Algorithm with tunable reaction rates for the reactions (B --> non-mutaded cell, M --> mutated cell)
B --> B + B // M --> M + M // B --> 0 // M --> 0 // B --> M // B + B --> B // M + M --> M

This is how it works:
Given a set of cultures (each culture is expected to have its own data file with one measurement per line: (time of measurement,  mutated cells at that time, total cells at that time)), and the time when the cultures were moved to a hostile environment, this code will calculate the probability distribution of the initial number of mutants at the moment of changing the medium (we will refer to this as N_0), given two measurements: the amount of cells at the beginning of the culture, T_0, and the amount of cells after a prudential time after the change of the medium was made (one where the growth is still exponential, but not too close so that we can safely say there are no non-mutated cells left), N_t. In order to do that, we use Bayesian inference methods.

We first calculate the priori distribution of each N_0 (via Luria-Delbruck distribution, which requires an initial guess of the mutation rate, which we can easily obtain via the p_0 method). Then, we calculate the likelihood, namely, the probability of having N_t mutated cells given that in the time of the change of the medium we had N_0. This probability is assumed to be Poisson-distributed. Then, we normalize the product.

Once we have the posteriori distribution, we consider two predictions for each N_0 for each culture: the N_0 which maximizes the posteriori and the average N_0 for that culture. Finally, two .txt are produced, one for the first type of prediction and another for the second. Finally, one just has to do a fitting test (not provided!) to a L-D distribution, for example, with the bz-rates web-app, http://www.lcqb.upmc.fr/bzrates. This allows the user to obtain a good estimator of the mutation rate.





- Why is it useful?
Eventhough in the example files HUNDREDS OF THOUSANDS of measurements are performed per simulation, there's really no need. As we said, just three measurements are needed per culture in order to obtain the mutation rate. That's, we believe, an improvement which can be useful.







- How to get started?
All you need is Python, an IDLE to modify the parameters, and basic libraries like numpy, matplotlib and pandas.

First, you need one .txt per culture. They have to be named "something_{number_of_culture}.txt". The format of each of these files should be (decimal point!)

time_of_measurement;total_cells_at_that_time




Then, you will need a file "were_there_mutants.txt" which includes a list of which cultures HAD MUTANTS with the following format:

2;5;7;8

(so only cultures 2, 5, 7 and 8, corresponding to the files "something_2.txt", "something_5.txt" etc developed mutants)



Then, you will need to launch "cells_at_time_of_change_of_medium.py". This code will receive the "something" part of the .txt files and the time when the change of medium was done. It will output another file called "cells_at_time_of_change_of_medium.txt" with the following format:

True/False(depending on whether there were mutants, this is obtained from the "were_there_mutants.txt");total_cells_at_time_of_antibiotic




Then, you need to execute the "main_code.py" code, which will ask for the birth rate, the death rate and the time of the measurement wayyy after the change of medium was performed. It will output two .txt files: one with the N_0 (number of mutants when the change of medium was added) guesses which maximize the posteriori and one with the average N_0 for each culture.


Finally, simply copy-paste that data to bz-rates and obtain your mu.




- Not sure how to use it?
Happy to help! Just send me a mail to oscdelfor.fausti@gmail.com and I'll try to assist as good as I can!
