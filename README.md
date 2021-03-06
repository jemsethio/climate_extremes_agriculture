# climate_extremes_agriculture
Analyse the effect of climate extremes on agricultural yields using Random Forests.


### Code for: The effects of climate extremes on global agricultural yields

This collection of scripts and functions was developed to analyse the effects of climate variations and climate extremes during the growing season on global agricultural yields of maize, rice, wheat and soy, using a Random Forests analysis. This notebook provides an example of the calculation based on one crop (maize).

The preparation of the growing season climate indices is described in: https://github.com/elisabethvogel/gs_clim_data


### Data used

#### Climate data

The following climate datasets are processed:

  1) CRU TS 3.23 dataset
  2) HadEX2 dataset 

##### References:

1) CRU TS 3.23

- URL: https://crudata.uea.ac.uk/cru/data/hrg/cru_ts_3.23/
- Paper: Harris, I., Jones, P.D., Osborn, T.J. and Lister, D.H. (2014), Updated high-resolution grids of monthly climatic observations – the CRU TS3.10 Dataset. Int. J. Climatol., 34: 623–642. doi: 10.1002/joc.3711

2) HadEX2

- URL: https://www.climdex.org/datasets.html
- Paper: Donat, M. G., L. V. Alexander, H. Yang, I. Durre, R. Vose, R. J. H. Dunn, K. M. Willett, E. Aguilar, M. Brunet, J. Caesar, B. Hewitson, C. Jack, A. M. G. Klein Tank, A. C. Kruger, J. Marengo, T. C. Peterson, M. Renom, C. Oria Rojas, M. Rusticucci, J. Salinger, A. S. Elrayah, S. S. Sekele, A. K. Srivastava, B. Trewin, C. Villarroel, L. A. Vincent, P. Zhai, X. Zhang and S. Kitching. 2013a. Updated analyses of temperature and precipitation extreme indices since the beginning of the twentieth century: The HadEX2 dataset, J. Geophys. Res. Atmos., 118, 2098–2118, http://dx.doi.org/10.1002/jgrd.50150.


#### Agricultural statistics

The agricultural yield, production and area harvested data was obtained from public databases by Ray et al. and is described in:

- Ray, D. K., Ramankutty, N., Mueller, N. D., West, P. C., and Foley, J. A. 2012. Recent patterns of crop yield growth and stagnation. Nat. Commun., 3, 1293


### Part 1: Data preparation

#### Step 1: Detrending climate and yield data

The climate and yield data were detrended using a non-linear, non-parametric detrending method based on Singular Spectrum Analysis (SSA).

```{r}
# 1) Detrending climate and yield data
source("code/data_preparation/01_detrend_climate_and_yield_data.R")
```


##### Step 2: Calculating standardised time series data

Grid cells with larger mean yields on average have larger yield anomalies (in absolute terms); the same for some of the climate variables. In order to be used in the same statistical model, the variance of the gridded time series of yield and climate anomalies (after detrending) was standardised by dividing by the standard deviation.


```{r}
source("code/data_preparation/02_calculate_standardised_timeseries.R")
```


#### Part 2: Read in all data and combine it

```{r}
crop = "maize"
statistical_method = "random_forest"
type_of_analysis = "full_model"

# 3a) Prepare a parameter list (equivalent to config file) with all information
# about the statistical analysis
source("code/data_preparation/03_function_create_parameter_list.R")
source("code/data_preparation/03_prepare_parameter_list.R") 

# 3b) Read in all data into a combined data frame for the statistical analysis
source("code/data_preparation/03_function_create_dataframe_for_statistical_analysis.R")
data_all = create_dataframe_for_statistical_analysis(parameter_list)
```

#### Part 3: Statistical analysis using Random Forest, including calculation of R2 values from cross-validated time series

```{r}
source("code/statistical_analysis/functions_statistical_analysis.R")
results = calculate_results_statistical_analysis(parameter_list, data_all = data_all,
                                       overwrite_data = FALSE)
```
