Note : Date Table is not availabe in the data set.
To Create a Date Table There are two ways. 1) Use Power Query paramenters to get dates and creat a table.
2) Use DAX function to create a Table. 

Both works and it's upto the user to use anyone.

```
Date = 
VAR MinDate = MINX(ALL('Listings'), 'Listings'[host_since])
VAR MaxDate = MAXX(ALL('Reviews'), 'Reviews'[Date])
RETURN
ADDCOLUMNS(
    CALENDAR(MinDate, MaxDate),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Day",FORMAT([Date],"DD"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Month Name", FORMAT([Date], "MMM"),
    "Week Number", WEEKNUM([Date], 2),  -- Week starts Monday
    "Week Day", FORMAT([Date], "dddd"),
    "Year-Week", "WK " & FORMAT(WEEKNUM([Date], 2), "00"),
    "Year-Month", FORMAT([Date], "YYYY'MMMM"),
    "Day Name", FORMAT([Date],"dddd"),
    "WeekEnd or WeekDay", IF(WEEKDAY([Date], 2) >= 6, "Week End", "Week Day"),
    "Review Year","Review"& YEAR([Date])
)

```
Below is Power Query M-codes along with steps:
1) Fetch latest and earliest date a single value out of given tables.
2) Use to Date value in a parameter.
3) Use this Parater to create Date Table based on your requirements of columns.

let
    StartDate = Earliest_Date, // Replace with your StartDate parameter name
    EndDate = Latest_Date,     // Replace with your EndDate parameter name
    DateList = List.Dates(
        StartDate,
        Duration.Days(EndDate - StartDate) + 1,
        #duration(1, 0, 0, 0)
    ),
    DateTable = Table.FromList(
        DateList,
        Splitter.SplitByNothing(),
        {"Date"}
    ),
    #"Add Year" = Table.AddColumn(DateTable, "Year", each Date.Year([Date])),
    #"Add Month" = Table.AddColumn(#"Add Year", "Month", each Date.Month([Date])),
    #"Add Date" = Table.AddColumn(#"Add Month", "Day", each Date.Day([Date])),
    #"Inserted Quarter" = Table.AddColumn(#"Add Date", "Quarter", each Date.QuarterOfYear([Date]), Int64.Type),
    #"Inserted Month Name" = Table.AddColumn(#"Inserted Quarter", "Month Name", each Date.MonthName([Date]), type text),
    #"Inserted Week of Year" = Table.AddColumn(#"Inserted Month Name", "Week of Year", each Date.WeekOfYear([Date]), Int64.Type),
    #"Inserted Start of Week" = Table.AddColumn(#"Inserted Week of Year", "Start of Week", each Date.StartOfWeek([Date],2), type date),
    #"Inserted Merged Column" = Table.AddColumn(#"Inserted Start of Week", "Year Month", each Text.Combine({Text.From([Year], "en-IN"), [Month Name]}, "-"), type text),
    #"Inserted Day Name" = Table.AddColumn(#"Inserted Merged Column", "Day Name", each Date.DayOfWeekName([Date]), type text),
    #"Added Conditional Column" = Table.AddColumn(#"Inserted Day Name", "Is Weekend or WeekDay", each if [Day Name] = "Sunday" then "Weekend" else if [Day Name] = "Saturday" then "Week End" else "Week Day"),
    #"Sorted Rows" = Table.Sort(#"Added Conditional Column",{{"Date", Order.Descending}})
in
    #"Sorted Rows"

```

```
Measures used in this Power BI:

Avg_Rating = AVERAGE('Listings'[review_scores_rating])
Avg_Score_by_Location = AVERAGE(Listings[review_scores_location])

Best District = 
VAR BestDist =
    SUMMARIZE(
        'Listings',
        'Listings'[district],
        "AvgScore", AVERAGE('Listings'[review_scores_location])
    )
VAR MaxScore = MAXX(BestDist, [AvgScore])
RETURN
    MAXX(
        FILTER(BestDist, [AvgScore] = MaxScore),
        'Listings'[district]
    )

Composite_Score = 
Var A= SUM(Listings[review_scores_checkin])
Var B= SUM(Listings[review_scores_communication])
RETURN
DIVIDE(A+B,2)

Listing_without_Reviews = COUNTROWS(DISTINCT('Listed_without_Review'))

Price = AVERAGE(Listings[price])

Reviews_2020 = CALCULATE(COUNT(Reviews[review_id]), YEAR(Reviews[date]) = 2020)

Total_Hosting = COUNTROWS('Listings')

Total_Reviews = COUNTROWS('Reviews')

Worst District = 
VAR LeastTable =
    SUMMARIZE(
        'Listings',
        'Listings'[district],
        "AvgScore", AVERAGE('Listings'[review_scores_location])
    )
VAR MinScore = MINX(LeastTable, [AvgScore])
RETURN
    MAXX(
        FILTER(LeastTable, [AvgScore] = MinScore),
        'Listings'[district]
    )

Review Year = YEAR(Reviews[date]) & " Review"

Listing_Age = DATEDIFF(Listings[host_since],TODAY(), YEAR) # this one is calculated columns.

```

Airnbn follows color codes which are not restricted to only use as it is. It's upto the creativity of the dashboard display any color combinations are matched.

Results:

1) There are nearly 30 % of listed hosts don't have reviews. Collecting reviews would throw insights on rating, price and potential growth or decline.

2) Highest Tourists footfall is in years 2016-2018. By excluding the 2020 pandemic results and trend analysis. More aminities try to attract more customers.

3) Airbnb has wide distribution and offer platform to differnt type of unique property types like Igloo, Islands, Old rock buildings etc, which can be utilized to advertise more.

4) Few more unique information would help to understand sales better like relative distance from nearest train, bus or airport, emergency services are missign to clearly corealate the ratings and checkin counts.
