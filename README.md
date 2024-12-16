[![INFORMS Journal on Computing Logo](https://INFORMSJoC.github.io/logos/INFORMS_Journal_on_Computing_Header.jpg)](https://pubsonline.informs.org/journal/ijoc)

# Learning-based Online Optimization for Autonomous Mobility-on-Demand Fleet Control

This archive is distributed in association with the [INFORMS Journal on
Computing](https://pubsonline.informs.org/journal/ijoc) under the [MIT License](LICENSE).

The software and data in this repository are a snapshot of the software and data
that were used in the research reported on in the paper 
[Learning-based Online Optimization for Autonomous Mobility-on-Demand Fleet Control](https://doi.org/10.1287/ijoc.2024.0637) by K.Jungel, A. Parmentier, M. Schiffer, and T. Vidal. 

The snapshot is based on 
[GitHub/ML-CO-pipeline-AMoD-control](https://github.com/tumBAIS/ML-CO-pipeline-AMoD-control) 
in the development repository. 

## Cite

To cite the contents of this repository, please cite both the paper and this repo, using their respective DOIs.

https://doi.org/10.1287/ijoc.2024.0637

https://doi.org/10.1287/ijoc.2024.0637.cd

Below is the BibTex for citing this snapshot of the repository.

```
@misc{JungelEtAl2024,
  author =        {Kai Jungel and Axel Parmentier and Maximilian Schiffer and Thibaut Vidal},
  publisher =     {INFORMS Journal on Computing},
  title =         {{Learning-based Online Optimization for Autonomous Mobility-on-Demand Fleet Control}},
  year =          {2024},
  doi =           {10.1287/ijoc.2024.0637.cd},
  url =           {https://github.com/INFORMSJoC/2024.0637},
  note =          {Available for download at https://github.com/INFORMSJoC/2024.0637},
}  
```

## Description

This software learns a dispatching and rebalancing policy for autonomous-mobility on demand systems using a structured learning enriched combinatorial optimization pipeline.

This repository contains all relevant scripts and data sets to reproduce the results from the paper.  
We assume using *slurm*.  
We used Python version 3.8.10.  
We used g++ version 9.4.0.  
We run the code on a Linux Ubuntu system.  
We thank Gerhard Hiermann to provide us the code from the paper [A polynomial-time algorithm for user-based relocation in free-floating car sharing systems](https://doi.org/10.1016/j.trb.2020.11.001) via git https://github.com/tumBAIS/kdsp-cpp. We used this code to calculate the k-dSPP solution.  

![](./visualization/movie_numVeh300_09-10.gif)


**This repository only contains script files while the [working repository](https://github.com/tumBAIS/ML-CO-pipeline-AMoD-control) also contains the raw data, input files, and result files.**


The structure of the repository is as follows:
- `cplusplus`: contains a C++ interface to run computationally intensive functions in C++
- `data`: contains all relevant data to reproduce the data
- `full_information_solutions`: contains all full_information solution instances. Please unpack the .zip folder.
- `learning_problem`: contains learning files to solve the structured learning problem
- `pipeline`: contains code which specifies the objects of the pipeline
- `prep`: contains code to preprocess the data
- `results`: contains the result directories in .zip format. Please unpack the .zip folders.
- `src`: contains helper files
- `visualization`: contains scripts to reproduce plots and gif
- `create_full_information_solution.py`: Script to solve a full-information problem
- `create_training_instances.py`: Script to rebuild a digraph solution from the full-information solution
- `evaluation.py`: Script to test benchmarks
- `master_script.sh`: Script to start creation of training data / training / evaluation
- `run_bash_benchmarks.cmd`: Script to evaluate benchmarks
- `run_bash_createInstances.cmd`: Script to rebuild digraph solution from full-information solution
- `run_bash_training.cmd`: Script to start training.py to train the *policy_SB* and *policy_CB*
- `run_pipeline.py`: Main file to process the pipeline
- `sanity_check.py`: Script to compare the Fenchel-Young loss and predictor loss
- `sanity_check_performance.py`: Script to compare the pipeline solution and the true solution
- `training.py`: Script to train the *policy_SB* and *policy_CB*

*Remark*: We use taxi trip data from https://www.nyc.gov/site/tlc/about/tlc-trip-record-data.page . We uploaded the taxi trip data we used to the working repository. 
Alternatively, you can download the data and store each day of trip data as a single .csv file in the `./data/taxi_data_Manhattan_2015_preprocessed/month-XX` directory. 
We specify the name and the format of the .csv file in `./data/taxi_data_Manhattan_2015_preprocessed/taxi_data_format.txt` .

Install dependencies with `pip install -r requirements.txt`.

## Overview
The execution of the code follows a three-step approach:
1. **Creation of training instances**: Calculation of full-information solution and rebuilding the digraph solution for online instances
2. **Training**: Minimization of structured learning loss
3. **Evaluation of learned policies**: *policy_CB* and *policy_SB* as well as benchmark policies *sampling*, *greedy*, and *full-information*.


## Creation of training instances
Specify the experiment in the *RUNNING_TYPE* variable in `master_script.sh`.  
To create training instances (full-information solution + generation of training instances), enable the line which calls the `run_bash_createInstances.cmd` script.  
All pre-specified parameters match the parameters used in the paper.  

## Training
Keep the same experiment in the *RUNNING_TYPE* variable in `master_script.sh` as for calculating the training instances.  
In the `master_script.sh` file, enable the line which calls the `run_bash_training.cmd` script.  
This script automatically trains the *policy_SB* and the *policy_CB* for the specified experiment.  
Then, run the `master_script.sh` file.

## Evaluation
The `run_bash_training.cmd` script automatically evaluates the performance of the training right after the training terminated.  
In the `run_bash_training.cmd` script you can define to evaluate the performance on a validation data set or on a testing data set. Both data sets are disjunct from the data set used for training.  

### Benchmarks
Keep the same experiment in the *RUNNING_TYPE* variable in `master_script.sh` as for training the *policy_SB* and the *policy_CB*.  
In the `master_script.sh` file enable the line which runs `run_bash_benchmarks.cmd`.  
In the `run_bash_benchmarks.cmd` script you can define to evaluate the performance on a validation data set or on a testing data set. Both data sets are disjunct from the data set used for training.  
Then, run the `master_script.sh` file.  
The script automatically evaluates the *offline*, *sampling*, and *offline* benchmark.  


## Visualization
Note: We uploaded the result files in the .zip format. Please first unpack the directories before running the visualization scripts.
- `visualization/visualization_results.py:` generates the plots from the paper
- `visualization/visualization_gif.py`: generates the gif presented on the top of this page
- `visualization/visualization_heatmap.py:` generates the heatmap from the paper showcasing the vehicle distribution for the different benchmarks (Requirement is to first run `visualization/visualization_gif`).

## Ongoing Development

This code is being developed on an on-going basis at the author's
[Github site](https://github.com/tumBAIS/ML-CO-pipeline-AMoD-control).

## Support

For support in using this software, submit an
[issue](https://github.com/tumBAIS/ML-CO-pipeline-AMoD-control/issues/new).
