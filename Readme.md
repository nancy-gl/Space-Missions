
# Space Mission Report

# Details of the Report
 
## Data Model:
  * The data in the flat file was transformed to a star schema model optimized for Power BI.
  * The model consists of space_missions fact and dates dimension tables.
  * The Data Model in Power BI looks like the following:
  
   ![](https://github.com/nancy-gl/Profitability-Analysis/blob/main/images/DataModel.png)
   
      
## Reporting and Visuals:
  * The focus of this project was to analyze Profitability across different dimensions.
  * A number of DAX measures were created for the analysis.
  * This project was an example of understanding of filter context, modifying filter context using CALCULATE, filtering the data with CALCULATE by using DAX functions like ALL, ALLSELECTED, REMOVEFILTERS and ALLEXCEPT.
  * Also applied conditional formatting within the visuals.
   
  * Calculated a number of DAX measures like [Total Sales], [Total Profit], [Average Profit], [Cumulative Sales], [Cumulative Profits] for the analysis.
  * The DAX used are as follows:
  
          * i.	Total Sales = SUM(Sales[SalesAmount])  
          * ii.	Total Profit = SUM(Sales[Profit])
          * iii. Cumulative Sales = CALCULATE( [Total Sales], FILTER( ALLSELECTED( Dates ), Dates[Date] <= MAX ( Dates[Date] )))
          * iv. Cumulative Profits = CALCULATE( [Total Profit], FILTER( ALLSELECTED( Dates ), Dates[Date] <= MAX ( Dates[Date] )))
          * v. Average Profit = AVERAGE(Sales[Profit])
          * vi. Dev from Average Profit = VAR AvgProfit = AVERAGE(Sales[Profit]) VAR AvgProfitAllSubcategory = CALCULATE( [Average Profit], ALL(Products[Sub-Category])) RETURN AvgProfit - AvgProfitAllSubcategory
                   
  * To analyse the profit and % profit by shipping mode from the baseline the following DAX measures were created.

         * i. % of Total Profit (Selected) = VAR TotalProfitALLSelected = CALCULATE( [Total Profit], ALLSELECTED(Sales)) RETURN DIVIDE([Total Profit], TotalProfitALLSelected, "-")
         * ii.	Total Profit (Allexcept) = CALCULATE( [Total Profit], ALLEXCEPT(Sales, Sales[Ship Mode]))
         * iii. Total Profit (ALL) = CALCULATE( [Total Profit], ALL(Sales))
         * iv. Baseline % by Mode = DIVIDE([Total Profit (Allexcept)], [Total Profit (ALL)], "-")
         * v. % of Total Profit (Selected) = VAR TotalProfitALLSelected = CALCULATE( [Total Profit], ALLSELECTED(Sales)) RETURN DIVIDE([Total Profit], TotalProfitALLSelected, "-")
         * vi. Variance from Baseline = [% of Total Profit (Selected)] - [Baseline % by Mode]
 
 * The DAX used for conditional formatting of the visuals are as follows:
  
          * i.	+ve/-ve formating = SWITCH( TRUE(), [Dev from Average Profit] >= 3, "#410891", [Dev from Average Profit] >= -30, "#9c94bd", [Dev from Average Profit] < 100, "#4d3dd6")  
          * ii.	MinMax State = VAR Vals = CALCULATETABLE( ADDCOLUMNS(SUMMARIZE( Sales, Locations[State] ), "@Profit" , [Total Profit]), ALLSELECTED()) VAR MinValue = MINX( Vals, [@Profit] ) VAR MaxValue = MAXX( Vals, [@Profit] ) VAR CurrentValue = [Total Profit] VAR Result = SWITCH( TRUE(), CurrentValue = MaxValue, 1, -- 1 for Max CurrentValue = MinValue, 2, -- 2 for Min CurrentValue < MaxValue && CurrentValue > MinValue, 0) RETURN Result
  
  * The Profitability Analysis Dashboard with the visuals is as shown:
  
  ![](https://github.com/nancy-gl/Profitability-Analysis/blob/main/images/Dashboard.png)


## [Click here to view the dashboard on the web.](https://app.powerbi.com/groups/ab67c13f-41ad-420a-a1bf-148ab4621cf3/reports/24d15c9c-e99f-45ca-ad55-b8b570114d30/ReportSectionb98ece9ddbe66491dec6)
