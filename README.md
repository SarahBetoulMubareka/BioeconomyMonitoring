# BioeconomyMonitoring
Repository to host R script for creating a composite indicator for monitoring the EU Bioeconomy
The code below shows the procedure for creating the composite view of trends of the EU Bioeconomy's main objectives. The approach is a hierarchical one, with treatment of indicators and aggregation to key component and normative criteria levels and ultimately to the Strategy Objectives level.
We start by exploring and cleaning the raw indicators and filling in data for the alternate years (some indicators report every other year only).
##Software used
R libraries 
library(purrr)
library(tidyverse)
library(dplyr)
library(COINr)
library(naniar)
library(COINr ) 

##Pre-processing the indicators
### Read all indicators csv file 
The indicators were bulk downloaded from the system internally. This is not yet possible from the Internet but is planned. The file is in this repository.

### Check available years in data 
We explore the years available for the data

### Delete unwanted rows
We know there are some redundant indicators (e.g. sum of parts) and we want to remove these. We also want to remove indicators that are from before 2008.Since we want to keep track of the rows deleted, these are listed one by one so they may be changed if data improves over time.

### Computing indicators at EU-27_2020 level  
To complete data coverage for all indicators we use values from the closest available year. Furthermore, we wish to compute the mean for the EU.
We selected the period between the years 2012 and 2017 for the analysis because this period contains data for all indicators. We apply the approach used by ESTAT for the computation of trends towards Sustainable Development Goals (https://ec.europa.eu/eurostat/web/sdi) . They apply a CAGR (Compound Annual Growth Rate) to two years, explained in Annex III of the ESTAT SDGs annual report for 2021. 
We apply this to the dataset, so we compute a new column with CAGR for each MS,then we compute the mean for the EU based on these numbers. The dataset has to be reshaped to wide format for the R package COINr to read. 
A key element to this work is the hierarchical structure of the EU Bioeconomy Monitoring System. We therefore have to introduce this to the system. The metadata and aggregate metadata files required by COINr are included in this repository. 

## Processing indicators 

### Statistics of raw data
To investigate the data better, we have a look at the statistics. This is done through ad hoc the COINr tools. The missing data points are identified at this stage, although we have already elimintated them from teh raw data at the very beggining. 

### Imputation
The indicators are imputed using indicator mean. In this particular example there are no empty data cells so this does nothing, but it is left in the code in case it ca be useful for other applications. 

### Data treatment for outliers 
Outliers can be useful or they can be a nuisance. It is up to the operator to decide this. COINr provides different solutions for treating outliers. For the Bioeconomy application, we use Winorisation. 

### Assembling the data and aggregating
The conceptual framework of the Monitoring System allows us to aggretage at different levels: Strategy Objective, Normative Criteria and Key Components. See Building a monitoring system for the EU bioeconomy: https://op.europa.eu/en/publication-detail/-/publication/9be6bf37-3e5e-11ea-ba6e-01aa75ed71a1 and
Development of a bioeconomy monitoring framework for the European Union: An integrative and collaborative approach :     https://pubmed.ncbi.nlm.nih.gov/32622862/

