# Bicilink
**A Sales Dashboard Power BI showing the performance of a bicycle company**

![image](https://user-images.githubusercontent.com/47380398/236808628-3b2996f5-a99a-4cbe-a4ee-584eec99f0cd.png)

The first page contains the analysis of the results for the year 2020 from January to June, included, comparing it with the same period of the previous year.
It is avoid if possible the use of calculated columns in order to get a better performance. The following are the DAX Measures used for the dashboard: 

  
- Revenue = SUMX( (Sales), Sales[Units] * Sales[Price] )   //  The total revenue for the total period (01/01/2018 to 06/12/2020) 
 
- YTD Revenue =  TOTALYTD( [Revenue], Sales[Date] )   // The Year-to-date revenue  
  


- Previous YTD Revenue = CALCULATE([YTD Revenue], SAMEPERIODLASTYEAR('Calendar'[DateKey])) // The previous Year-to-date revenue 

- YOY Difference =  DIVIDE([YTD Revenue],[Previous YTD Revenue]) -1  // The Year over year difference showed as a percentage

- Running Total Revenue = 
    CALCULATE( 
        'DAX Measures'[Revenue], 
        DATESBETWEEN( 
            'Calendar'[DateKey], 
            STARTOFYEAR('Calendar'[DateKey]),
            MAX('Calendar'[DateKey])
        )
    )  // The running total revenue used to see the progress towards a revenue goal
 
 - Ranking by product inside a category = 
    IF(
        ISINSCOPE( Products[Product] ),
        RANKX(
            CALCULATETABLE(
                VALUES( Products[Product] ),
                ALLSELECTED( Products[Product] )
            ),
            [Revenue]
        )
    )  // DAX measure to show the rank of the products according to the revenue
    
![image](https://user-images.githubusercontent.com/47380398/236815426-b213fe08-1204-44b8-838e-ac13469f6c5b.png)

The second page contains the analysis of the Products Performance, where the user can filter by year (2018,2019 and/or 2020), by product category and by manufacturer.

In order to achieve consistency between years, as the 2020 has only full data fo all the countries from January to June, a filter page was applied, showing sales from 01/01/2018 to 30/06/2020.
Also a DAX measure was used to synchronize product and manufacturer slicers (filters) to guarantee that if the user select a category, the manufacturer slicer only shows those who make bicycles in that category:  
Slicer Manufacturer = 
  INT ( NOT ISEMPTY(Sales) )
