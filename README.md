Python2Simulink
===============
#### Robotics and Intelligent Vehicle Automation Lab (RIVAL)
- Built by Dong Chen
- Started on Jan.11, 2020

A bridge between Python and Simulink.
This file aims to build a bridge between Python and Simulink. At each time step, the python script will send a command (input) to the simulink model, then the simulink model executes for one step and then returns the results to Python scripts for the decision usage. 

## Install MATLAB Engine API for Python
-------

Install the MATLAB Engine API follow the instruction [Installation](https://www.mathworks.com/help/matlab/matlab_external/install-the-matlab-engine-for-python.html).

## Applications
-------

1. plant example

In this example, we build a PI controller to regulate a secord-order system to a reference value (10 here). The Python script compute the control input and sends then value to the Simulink model. Then Simulink model runs for one step and returns the output value to the python script. 

<p align="center">
     <img src="Docs/plant.gif" alt="output_example" width="60%" height="60%">
     <br>Fig.1 Figure of regulation result
</p>

2. tracking example

In this example, the controller tries to control the variables x1 and x2 to a regulate track xd1 and xd2. There is also a code-like model and controller built by [Matlab code](examples/tracking/linear_controller.m).

The simulink model looks like this,

<p align="center">
     <img src="Docs/simulink_model.png" alt="output_example" width="60%" height="60%">
     <br>Fig.2 simulink model
</p>

where the u1 and u2 are control input from the Python script. The state variables x1 and x2 will be sent to the python script. 

<p align="center">
     <img src="Docs/tracking.gif" alt="output_example" width="60%" height="60%">
     <br>Fig.3 Tracking result, x1(blue) and xd1(orange)
</p>

3. PV Control

This model is based on the Tutorial [here](https://www.youtube.com/watch?v=A9FhgHS1JsE).

## Gym-like Reinforcement Learning interface combined with Python and Matlab

The Gym-like environment file is located in [IQL_conventional](IQL_conventional/) folder:

- [env](DDQN_Ford/env/) folder contains the environment file.
- [config](DDQN_Ford/config/) folder contains the system setting for RL model and environment.
- [agent](DDQN_Ford/agents/) folder contains the RL model and policies.
 
 
## API
-------

- start the engine and connect to Matlab; Load the model:

```
matlab.engine.start_matlab()
self.eng.eval("model = '{}'".format(self.modelName),nargout=0)
self.eng.eval("load_system(model)",nargout=0)
 ```

- eng.eval('out.output')

- eng.workspace()

- self.eng.set_param('{}/u'.format(self.modelName),'value',str(u),nargout=0): set the control input u.

- Start Simulation and then Instantly pause and read output.

```
eng.set_param(self.modelName,'SimulationCommand','start','SimulationCommand','pause',nargout=0)
out = self.eng.workspace['out']
self.eng.eval('out.output'), self.eng.eval('out.tout')
```

- set_param

learn how to set parameters of blocks [check here](https://www.mathworks.com/help/simulink/slref/set_param.html)

-update on 18/5/24
for my case i am trying tracking example

1) go to the tracking file
2) Open tracking.py with python 3.11 (or 3.9 and 3.10) version interpreter ( i am using pycharm IDE)
3) You should check the python version first before proceed, very important.
4) run your command prompt as administrator.
5) cd "C:\Program Files\MATLAB\R2023a\extern\engines\python" (refer on your own)
6) python setup.py install
7) one of the file will contain "matlab" file, copy this to the downloaded file\tracking.
8) go to terminal
9) pip install matplotlib
10) python -m pip install matplotlib
11) run your python script.
12) Done.

## Reference:
-------

1. [link1](https://stackoverflow.com/questions/48864281/executing-step-by-step-a-simulink-model-from-python)
2. [Calling MATLAB from Python](https://www.mathworks.com/help/matlab/matlab-engine-for-python.html)
3. [Troubleshoot MATLAB Errors in Python](https://www.mathworks.com/help/matlab/matlab_external/troubleshoot-matlab-errors-in-python.html)
4. [Call User Scripts and Functions from Python](https://www.mathworks.com/help/matlab/matlab_external/call-user-script-and-function-from-python.html)
5. [usage of eng.set_param](https://www.mathworks.com/help/simulink/slref/set_param.html)
