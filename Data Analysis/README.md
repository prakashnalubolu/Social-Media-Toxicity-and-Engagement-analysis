[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/MolMMnYF)



## Steps to get the analysis plots:
1) Firstly, make sure all of the docker containers are up and running. Hopefully it should.
2) The .env files are not pushed to github. Hence if you wanted to clone the project and 
try running the project, you may wanted to create your own docker containers and enter the 
respective values in .env files
3) If you login into the VM with your previleged access, then you can directly run the python notebooks from nect nextion to generate the plots. 
4) The data is queried from the database everytime when you run the below notbooks and 
plots are generated with all the data that has been collected so far. Hence, the plots 
generated here might differ a bit from the plots that are included in the report. 
5) Eventhough the python notebooks have more plots, only part of it were included in the report

## Python notebooks with plots
- 4chan: Run all the cells of 4chan_crawler/analysis.ipynb
- Reddit: Run all the cells of reddit_crawler/analysis_1.ipynb
- Analysis common to 4chan and reddit: Run all the cells in common_analysis/analysis.ipynb