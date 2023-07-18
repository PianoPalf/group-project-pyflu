# PyFlu - Group 2

Analysis of factors that influence the transmission of influenza globally before, during and after the COVID-19 pandemic.

## Table of Contents

- [General info](#general-info)
- [Technologies](#technologies)
- [Logic of Code](#logic-of-code)
- [Screenshot](#screenshot)
- [Code Examples](#code-examples)
- [Summary & Conclusions](#summary-&-conclusions)
- [Authorship](#authorship)
- [References](#references)

## General info

- Reads data from .csv files acquired from multiple sources.
- Cleans data and generates summary Pandas DataFrames for subsequent analyses.
- Generates Chloropleth maps of influenza cases globally over multiple years for visualisation purposes. The time period analysed spans before, during and after the COVID-19 pandemic.
- Plots corresponding line graphs for specific countries of interest as gleaned from Chloropleth maps.
- Analyses and plots influenza cases by seasons for multiple countries.
- Analyses and generates plots with respect to GDP, capital health expenditure and general hygiene standards to identify potential correlations.

## Technologies

Project created and run using:

- Python 3.10.9
  - Pandas 1.5.3
  - Plotly 5.15.0
  - NumPy 1.24.3
  - Matplotlib 3.7.1
  - SciPy 1.10.1

- Visual Studio Code 1.79.2
- Jupyter Notebook 5.3.0

## Logic of Code



## Screenshot

#### Chloropleth Map with Year Slider:

![chloropleth_map_slider](/Users/samuelpalframan/Jupyter Notebook/Module_7_Group_Project/GitHub/group-project-pyflu/Resources/chloropleth_map_slider.png)

## Code Examples

#### Cleaning of Data and Generation of Pandas DataFrames:

```python
# Code Snippet from PyFlu_dataframes_SP.ipynb

# Change multiple Year Columns to one Column, to match 'influenza_df'
populations_one_column_df = populations_df.melt(id_vars=["Country Name", 'Country Code'], 
                                        var_name="Year", 
                                        value_name="Population")

# Merge all three dataframes together
flu_gdp_pop_df = influenza_df.merge(gdp_one_column_df,on=['Country Code', 'Country Name', 'Year'])\
                             .merge(populations_one_column_df, on=['Country Code', 'Country Name', 'Year'])
```

#### Generation of Chloropleth Map with Year Slider:
```python
# Code Snippet from PyFlu_maps_slider_SP.ipynb

# Loop through each Year for Influenza Cases
for i in years:
    
    # Create temp_df for each Year
    temp_df = flu_gdp_df.loc[flu_gdp_df['Year'] == i]
    
    # Generate data to plot on Chloropleth map
    data = [dict(type ='choropleth',
                 
                 # Give three-letter country code to enable plot for each country
                 locations = temp_df['Country Code'],
                 
                 # Text shown when hovering over each country
                 text = temp_df['Country Name'],
                 
                 # Set data to Display; Max/Min values and title for Scale Bar
                 z = temp_df['Cases (per million people)'],
                 zmax = max_cases,
                 zmin = min_cases,
                 colorbar=  dict(title='cases per million'),
                 
                 # Set location mode to three-letter country code
                 locationmode = 'ISO-3', 
                 
                 # Make Scale bar values only visible for one year at a time
                 visible = True if i == years[-1] else False)] 
    
    # Store data in List created above
    cases_per_year.extend(data)
```

## Summary & Conclusions 

#### Effect of COVID-19 on the number of influenza cases

It was found that there were less reported influenza cases during the COVID-19 pandemic. This could be attributed to the effect of 'lockdowns' during the pandemic, as lockdowns drastically reduce person-person contact and therefore the transmission of airborne viruses. The phenomenon might also be explained by a reduced capacity for a given country to test for influenza during this time, as COVID-19 overwhelmed the healthcare systems of both developed and developing countries.

## Authorship

Created and submitted as Group Project for Monash University Data Analytics Boot Camp (July 2023).

Data collated, cleaned and code written by:

- Caroline Grant
- Giang Nguyen
- Samuel Palframan
- Eric Tran

## References

### Data Sources:
- Influenza data (WHO):
	- https://www.who.int/teams/global-influenza-programme/surveillance-and-monitoring/influenza-surveillance-outputs
- GDP data (World Bank):
	- https://www.worldbank.org/en/home
- Population data (World Bank):
  - https://www.worldbank.org/en/home

### Code Adaptations
- Chloropleth Map:
  - https://www.datacamp.com/tutorial/making-map-in-python-using-plotly-library-guide
- Chloropleth Map with Slider for Years:
	- https://stackoverflow.com/questions/76143436/plotly-choropleth-sliders-break-after-converting-to-html-with-nbconvert
