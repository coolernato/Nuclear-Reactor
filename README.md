# Nuclear Reactor Example Project

This example project involves simulating a nuclear reactor in a very simplified way.
The population of neutrons and dealyed neutron precursors, fuel temeprature and coolant temperature are simulated.
The reactor is modeled as a single discretised vertical channel.
The equation solved are outlined in the Equations notebook.
The code is written in Python and heavily uses Numpy and Scipy.

## Key Skills

The following key skills are used in this project:

* Python
* Reading from a file
* Numpy and Scipy
* Initial Value Problems
* Matplotlib

## Project Overview

The following files are found in the project:

* inputs/sample1: A sample input file
* constant_reactivity: A description of a constant reactivity
* derivative: A function which defines the rate of change of the different variables which describe the state of the system as a function of the current state of the system
* future_states: A function which calculates and returns future states of the system based on an initial state of the system
* input_reader: Functions which reads and input file and constructs a specification of the problem
* nuclear_reactor: The main file which calls various other functions and plots the output of the simulation
* problem_specification: a class which contains a specification of the problem being solved
* ramp_reactivity: A description of a reactivity as a function of time which is initially stable, then changes linearly, then holds constant
* state_variables: A class which holds the main variables being solved for - the ones which are solved for using the main equations
* state: A class which inherits from StateVariables which also contains a variety of properties which calcualte useful values of the state of the system

## Points of Interest

* Both the ConstantReactivity and RampReactivity classes implement the "\_\_call\_\_" magic method, allowing instances of those classes to be called like a function.
* The ProblemSpecification class includes the member variable "\_reactivity_driving" which may be an instance of ConstantReactivity or RampReactivity. In this way, this variable and the code which uses it is polymorphic.
* ProblemSpecification is a class whose instances are designed to approximate immutability - the constructor creates variables which are intended to be treated as private (indicated by the preceding "\_") and are accessed by properties with no associated setters.
* StateVairables is a class designed to hold the variables which are directly solved for by the Orindary Differential Equation (ODE) solver. It contains methods to populate it from an array or to populate an array using its data. This allows it to be used to conveniently store data in the "derivative" function for the rate of change of the function by accessing variables like "gradient.t_fuel", rather than having to work out which element of the array to alter. Once the "gradient" variable has been populated, an array can be created to be returned and used by "odeint".
* State is a class which inherits from StateVariables and so also has this functionality, allowing data to be accessed from naturally-named variables like "state.n_neutron" in "derivative" once the "state" variable has been populated from the array passed in. It also contains a number of properties which calculate useful values of the current state.
* "get_reactivity" and "get_value" contain some moderately complex logic including loop control, error handling and have a "for" loop with an "else" statement which is executed if the loop is not ended by a "break" or "return" statement.