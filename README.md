# Crime in Detroit

## Introduction

Starting from the data on 911 calls, the project examines violent crimes committed in the city of Detroit over the past thirty days and their spatial distribution among the city's neighborhoods. It is hypothesized that the frequency of crimes is higher in proximity to certain categories of facilities. For example, it is more likely that a robbery will occur if you are near a bank. Conversely, it is also possible to hypothesize that there are facilities that serve as deterrents. For instance, the highest concentration of cultural and social services, as well as the presence of police stations, could lead to a reduction in crimes in their vicinity.

The data used for this study is available on Detroit's Open Data Portal and Data Driven Detroit. Data on facilities is downloaded from Open Street Maps. Every time the code is executed, the most up-to-date data is downloaded.

In this notebook, a geospatial exploration of the data with the visualization of informative maps is realized, followed by data processing, as well as the calculation of the distance between the locations of dangerous events and the nearest police station. Additionally, for each category of facility, the distance from the nearest one is calculated.

Furthermore, data on 911 calls are filtered as follows: only received calls (not made by operators) with the highest priority (emergency situations) and for which the neighborhood is known are considered.

Lastly, for informative purposes, two categories of violent crimes have been manually identified: those committed against property and those committed against individuals. Other categories, such as those related to traffic violations or mental health issues, are not considered for the current study.

## Data Analysis Results

The Moran's I test was significant at the 0.05 level, indicating spatial autocorrelation in the number of crimes between neighborhoods.

Spatial regression modeling was conducted to account for the complex spatial dynamics of crime distribution in Detroit, particularly in relation to various types of facilities. This approach helps to understand how the presence or absence of certain facilities impacts crime rates in nearby areas, considering both the attraction and deterrent effects. The model aims to accurately capture these spatial dependencies, allowing for a nuanced understanding of urban crime patterns and informing effective urban planning and public safety strategies.

In conducting spatial regression, it's essential to understand whether the phenomenon under study is likely to generate local spatial spillovers or global spatial spillovers. Results suggest a local spatial spillover, as SDM was the model fitting the data the best. 


### Spatial Durbin Model (SDM) Results

<table class="table table-striped table-hover">
    <thead>
        <tr>
            <th>Term</th>
            <th>Estimate</th>
            <th>Std. Error</th>
            <th>z value</th>
            <th>Pr(&gt;|z|)</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>(Intercept)</td>
            <td>14.7849</td>
            <td>6.8375</td>
            <td>2.1623</td>
            <td>0.030594</td>
        </tr>
        <tr>
            <td>education</td>
            <td>2.4041</td>
            <td>3.6103</td>
            <td>0.6659</td>
            <td>0.505466</td>
        </tr>
        <tr>
            <td>food_drink</td>
            <td>-4.6330</td>
            <td>5.1441</td>
            <td>-0.9006</td>
            <td>0.367779</td>
        </tr>
        <tr>
            <td>healthcare</td>
            <td>-1.1928</td>
            <td>3.2861</td>
            <td>-0.3630</td>
            <td>0.716610</td>
        </tr>
        <tr>
            <td>finance</td>
            <td>-2.6204</td>
            <td>3.1110</td>
            <td>-0.8423</td>
            <td>0.399617</td>
        </tr>
        <tr>
            <td>socio_cult</td>
            <td>-6.4765</td>
            <td>3.2110</td>
            <td>-2.0169</td>
            <td>0.043703</td>
        </tr>
        <tr>
            <td>police_sta</td>
            <td>8.0499</td>
            <td>2.8560</td>
            <td>2.8186</td>
            <td>0.004823</td>
        </tr>
        <tr>
            <td>lag.education</td>
            <td>-9.9946</td>
            <td>5.6008</td>
            <td>-1.7845</td>
            <td>0.074342</td>
        </tr>
        <tr>
            <td>lag.food_drink</td>
            <td>4.4518</td>
            <td>9.2711</td>
            <td>0.4802</td>
            <td>0.631096</td>
        </tr>
        <tr>
            <td>lag.healthcare</td>
            <td>1.3787</td>
            <td>5.3420</td>
            <td>0.2581</td>
            <td>0.796337</td>
        </tr>
        <tr>
            <td>lag.finance</td>
            <td>3.5008</td>
            <td>4.9826</td>
            <td>0.7026</td>
            <td>0.482301</td>
        </tr>
        <tr>
            <td>lag.socio_cult</td>
            <td>10.2556</td>
            <td>4.2702</td>
            <td>2.4016</td>
            <td>0.016322</td>
        </tr>
        <tr>
            <td>lag.police_sta</td>
            <td>-8.6830</td>
            <td>3.4684</td>
            <td>-2.5034</td>
            <td>0.012299</td>
        </tr>
    </tbody>
</table>

### Additional Model Statistics

- **Rho:** 0.25751
- **LR test value:** 7.8703, **p-value:** 0.0050254
- **Asymptotic standard error:** 0.099796
- **z-value:** 2.5804, **p-value:** 0.0098683
- **Wald statistic:** 6.6585, **p-value:** 0.0098683
- **Log likelihood:** -831.7507 for mixed model
- **ML residual variance (sigma squared):** 334.72, (sigma: 18.295)
- **Number of observations:** 192
- **Number of parameters estimated:** 15
- **AIC:** NA (not available for weighted model), (AIC for lm: 1699.4)
- **LM test for residual autocorrelation test value:** 2.5614, **p-value:** 0.1095

## Conclusions

Proximity to police stations may have a deterrent effect on crime. On the contrary, greater distances to socio-cultural facilities correlate with a decrease in crime rates. This could imply that also closer proximity to socio-cultural amenities may be associated with lower crime rates.
The significant positive coefficient for lagged socio-cultural distances suggests that if neighboring areas are further from socio-cultural facilities, the focal area may experience higher crime rates.  Conversely, greater distances to police stations in neighboring areas correlate with lower crime rates in the focal area. This indicates a spill-over effect where the benefits of proximity to police stations extend beyond immediate neighborhoods.

