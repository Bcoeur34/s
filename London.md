![tower bridge](london.jpg)

London, or as the Romans called it "Londonium"! Home to [over 8.5 million residents](https://www.ons.gov.uk/peoplepopulationandcommunity/populationandmigration/populationestimates/bulletins/populationandhouseholdestimatesenglandandwales/census2021unroundeddata#population-and-household-estimates-england-and-wales-data) who speak over [300 languages](https://web.archive.org/web/20080924084621/http://www.cilt.org.uk/faqs/langspoken.htm). 
While the City of London is a little over one square mile (hence its nickname "The Square Mile"), Greater London has grown to encompass 32 boroughs spanning a total area of 606 square miles! 

![underground train leaving a platform](tube.jpg)

Given the city's roads were originally designed for horse and cart, this area and population growth has required the development of an efficient public transport system! Since the year 2000, this has been through the local government body called **Transport for London**, or *TfL*, which is managed by the London Mayor's office. Their remit covers the London Underground, Overground, Docklands Light Railway (DLR), buses, trams, river services (clipper and [Emirates Airline cable car](https://en.wikipedia.org/wiki/London_cable_car)), roads, and even taxis.

The Mayor of London's office make their data available to the public [here](https://data.london.gov.uk/dataset). In this project, you will work with a slightly modified version of a dataset containing information about public transport journey volume by transport type. 

The data has been loaded into a **Snowflake** database called `TFL` with a single table called `JOURNEYS`, including the following data:

## TFL.JOURNEYS

| Column | Definition | Data type |
|--------|------------|-----------|
| `MONTH`| Month in number format, e.g., `1` equals January | `INTEGER` |
| `YEAR` | Year | `INTEGER` |
| `DAYS` | Number of days in the given month | `INTEGER` |
| `REPORT_DATE` | Date that the data was reported | `DATE` |
| `JOURNEY_TYPE` | Method of transport used | `VARCHAR` |
| `JOURNEYS_MILLIONS` | Millions of journeys, measured in decimals | `FLOAT` |

Note that *in Snowflake all databases, tables, and columns are **upper case*** by default.

You will execute SQL queries to answer three questions, as listed in the instructions.


```python
-- most_popular_transport_types
SELECT JOURNEY_TYPE, 
	SUM(JOURNEYS_MILLIONS) as TOTAL_JOURNEYS_MILLIONS
FROM TFL.JOURNEYS
GROUP BY JOURNEY_TYPE
ORDER BY TOTAL_JOURNEYS_MILLIONS DESC;
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>JOURNEY_TYPE</th>
      <th>TOTAL_JOURNEYS_MILLIONS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bus</td>
      <td>24905.193947</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Underground &amp; DLR</td>
      <td>15020.466544</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Overground</td>
      <td>1666.845666</td>
    </tr>
    <tr>
      <th>3</th>
      <td>TfL Rail</td>
      <td>411.313421</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Tram</td>
      <td>314.689875</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Emirates Airline</td>
      <td>14.583718</td>
    </tr>
  </tbody>
</table>
</div>




```python
-- emirates_airline_popularity
SELECT MONTH, 
	YEAR, 
	ROUND(JOURNEYS_MILLIONS,2) AS ROUNDED_JOURNEYS_MILLIONS
FROM TFL.JOURNEYS
WHERE JOURNEY_TYPE = 'Emirates Airline' AND ROUNDED_JOURNEYS_MILLIONS IS NOT NULL
ORDER BY ROUNDED_JOURNEYS_MILLIONS DESC
LIMIT 5;
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>MONTH</th>
      <th>YEAR</th>
      <th>ROUNDED_JOURNEYS_MILLIONS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>5</td>
      <td>2012</td>
      <td>0.53</td>
    </tr>
    <tr>
      <th>1</th>
      <td>6</td>
      <td>2012</td>
      <td>0.38</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4</td>
      <td>2012</td>
      <td>0.24</td>
    </tr>
    <tr>
      <th>3</th>
      <td>5</td>
      <td>2013</td>
      <td>0.19</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>2015</td>
      <td>0.19</td>
    </tr>
  </tbody>
</table>
</div>




```python
-- least_popular_years_tube
SELECT YEAR,
	JOURNEY_TYPE,
	SUM(JOURNEYS_MILLIONS) as TOTAL_JOURNEYS_MILLIONS
FROM TFL.JOURNEYS
WHERE JOURNEY_TYPE LIKE '%Underground%'
GROUP BY YEAR, JOURNEY_TYPE
ORDER BY TOTAL_JOURNEYS_MILLIONS
LIMIT 5;
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>YEAR</th>
      <th>JOURNEY_TYPE</th>
      <th>TOTAL_JOURNEYS_MILLIONS</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2020</td>
      <td>Underground &amp; DLR</td>
      <td>310.179316</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2021</td>
      <td>Underground &amp; DLR</td>
      <td>748.452544</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022</td>
      <td>Underground &amp; DLR</td>
      <td>1064.859009</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2010</td>
      <td>Underground &amp; DLR</td>
      <td>1096.145588</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2011</td>
      <td>Underground &amp; DLR</td>
      <td>1156.647654</td>
    </tr>
  </tbody>
</table>
</div>


