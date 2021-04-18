---
title:  "Perform optimal LQR using MATLAB reinforcement learning agents"
last_modified_at: 2021-04-18
categories: 
  - Reinforcement Learning
tags:
  - MATLAB
  - RL
  - LQR
toc: true
toc_label: "LQR"
comments: true
---

MATLAB help document: [`link`](https://ww2.mathworks.cn/help/reinforcement-learning/ug/custom-agents.html)  

## Problem Formulation 

Given a LQR optimal control problem formulated in the following discrete form:

$$
x_{t+1}=Ax_t+Bu_t
$$

And a LQR evaluation function:

$$
J ( x_t, u_t) =\sum^\infty _{t=0} (x'_t Q x'_t + u'_t R u_t)
$$

The feedback control law is:

$$
u_t = -K x_t
$$

We want the optimal control gain such that the evaluation function is minimized. 

## Implementation 

### Define a customized MATLAB LQR agent. 

1. Create a template from the file "LQRCustomAgent.m"

```matlab
addpath(fullfile(matlabroot,'examples','rl','main')); % add the template path
edit LQRCustomAgent.m;	% get the template file
```
   
2. Save the template file to the work directory. Move remove the example files for safety.

```matlab
rmpath(fullfile(matlabroot,'examples','rl','main')); 
```

### Define a customized reinforcement learning environment 

The file "myDiscreteEnv.m" can be found in examples.

### Generate an instance and training 

## Main Code -- Code 1

```matlab
%This simulation is a blog code
%Created by Zejian Zhou @ UNR/EE Autonomous Systems Lab
%Email: zhouzejian1994@gmail.com

A = [1.05,0.05,0.05;0.05,1.05,0.05;0,0.05,1.05];
B = [0.1,0,0.2;0.1,0.5,0;0,0,0.5]; 

Q = [10,3,1;3,5,4;1,4,9]; 
R = 0.5*eye(3);

env = myDiscreteEnv(A,B,Q,R);
rng(0)

K0 = place(A,B,[0.4,0.8,0.5]);
agent = LQRCustomAgent(Q,R,K0);

agent.Gamma = 1;
agent.EstimateNum = 45;

trainingOpts = rlTrainingOptions(...
    'MaxEpisodes',10, ...
    'MaxStepsPerEpisode',50, ...
    'Verbose',true, ...
    'Plots','none');

trainingStats = train(agent,env,trainingOpts);
```

