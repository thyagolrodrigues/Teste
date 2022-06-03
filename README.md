# ![COVID_Data_Exploration_with_SQL_and_Power_BI](https://user-images.githubusercontent.com/105753824/171853676-ff15a500-4f77-40c8-a4a5-ab06c566b0f2.png)

Below I will walk you through each step of this project. I want to showcase my SQL and Power BI skills, as well as show my organizational process and logical thinking when it comes to data projects, so future employers may have an idea of how I work.

If you want to see only the SQL and Power BI part, you can skip to the Exploratory Data Analysis by [clicking here](#exploratory-data-analysis)<br/>

# Summary

* [Reason of this project](#reason-of-this-project)<br/>
* [Choosing the database](#choosing-the-database)<br/>
* [Data extraction](#data-extraction)<br/>
* [Exploratory Data Analysis](#exploratory-data-analysis)<br/>
* [Power BI:](#power-bi)<br/>
&ensp;&ensp;&ensp;&ensp;* [Data Model](#data-model)<br/>
&ensp;&ensp;&ensp;&ensp;* [DAX](#dax)<br/>
&ensp;&ensp;&ensp;&ensp;* [Dashboard](#dashboard)<br/>
* [Conclusion](#conclusion)<br/>

# Reason of this project:
Currently (May 2022), COVID occurrences are not the same as a few months ago, so I was wondering what countries were impacted the most by the virus.

There are several ways to find out that - news, online dashboards, etc. However, I wanted to get to these answers by manipulating and organizing data myself, so I can also include this in my portfolio for future employers to see.

The idea is to answer the below questions by the end of this project:
- Total number of cases/deaths globally;
- Population infected;
- Mortality; and
- Number of people vaccinated as well as the percentage that it represents

It is important to highlight that there are several indicators that could be analyzed. Nevertheless, the idea of this project is not to go in-depth. I will just work on the above points.

# Choosing the database:
I chose the COVID-19 database that is available on the Our World in Data website, which is a serious source that I know and have used before for other analyses.

# Data extraction:
The database comes in the CSV format, but the idea is to use SQL. And to make things more interesting, I divided the main file into two different files, one with the cases/deaths data, and the other with the vaccination data. Consequently, I must use more functions in SQL (LEFT JOIN, for instance), so I have more knowledge to showcase. Lastly, I imported each file as a table in a SQL database that I created, named PortfolioProject_COVID.

# Exploratory Data Analysis:
The databases from Our World in Data come well-organized, so this was not a project that had much cleansing/transformation, but there will be others in my portfolio to showcase my ability to do that.

Below you can see the final query that I created the view to use in Power BI. If you want to see how I got to this, please click on "Click here to see the whole code" located right after the code box, so you will see each step (initial look, understanding the columns, etc).

``` sql

CREATE VIEW DeathsAndVaccinations AS
SELECT
	death.continent,
	death.location,
	death.date,
	death.population,
	death.new_cases,
	death.new_deaths,
	vaccinations.people_fully_vaccinated
FROM
	PortfolioProject_COVID..CovidDeaths AS death
LEFT JOIN PortfolioProject_COVID..CovidVaccinations AS vaccinations
	ON death.location = vaccinations.location
	AND death.date = vaccinations.date
WHERE
	death.continent IS NOT NULL
```

<details>
  <summary>
    Click here to see the whole code
      </summary>
	
``` sql
	
-- Initial look at the complete Deaths table: See how it is set, see the columns, and the data:

SELECT
	*
FROM
	PortfolioProject_COVID..CovidDeaths

-- Initial look at the complete Deaths table: Ordering by continent and country, for better visualization. Also, I could notice that the column continent has many lines as NULL, so let's investigate the reason.

SELECT
	*
FROM
	PortfolioProject_COVID..CovidDeaths
ORDER BY
	continent ASC,
	location ASC
WHERE
	continent is NULL

-- I want to look at the distinct values of the column location when filed by continent NULL

SELECT DISTINCT
	location
FROM
	PortfolioProject_COVID..CovidDeaths
WHERE
	continent is NULL
ORDER BY
	location ASC

-- Initial look at the complete Deaths table: Ordering by continent and country, and considering only the NOT NULL for the column continent, as I noticed that these lines have another classification (such as High Income, Low Income, etc) instead of the locations themselves

SELECT
	*
FROM
	PortfolioProject_COVID..CovidDeaths
WHERE
	continent IS NOT NULL
ORDER BY
	continent ASC,
	location ASC,
	date ASC

-- I performed the same steps above for the table Vaccinations, and it has the same issues with the continent in NULL

-- Selecting the columns that I will bring to Power BI for the purpose that we have in this project. As this query results 176.870 lines, I will not add the calculations here, because I believe that DAX in Power BI will perform better than adding 176.870 lines for each column that would be added. Also, I will use LEFT JOIN to get the columns that I will need from the table Vaccinations.

-- This is the same query from the beginning:

SELECT
	death.continent,
	death.location,
	death.date,
	death.population,
	death.new_cases,
	death.new_deaths,
	vaccinations.people_fully_vaccinated
FROM
	PortfolioProject_COVID..CovidDeaths AS death
LEFT JOIN PortfolioProject_COVID..CovidVaccinations AS vaccinations
	ON death.location = vaccinations.location
	AND death.date = vaccinations.date
WHERE
	death.continent IS NOT NULL
	
```
</details>
    

# Power BI:

## Data Model
As there is only one table, the only relationship that there will be is between the COVID-19 database and the Calendar table (created by me in Power BI). Further, I normally create a new table to add all the measures created, I think it is better to organize this way 

 ![Model](https://user-images.githubusercontent.com/105753824/171749957-cedf60f4-7f6a-4e10-af79-1886e81f0a53.jpg)

## DAX
Creation of the calendar table, and some measures for the visualization. The measures were relatively simple, the only one that got a bit more challenging was the one to define the number of people fully vaccinated.

![Medidas](https://user-images.githubusercontent.com/105753824/171749997-631780fe-35c4-4fda-b575-8fa8ed656638.jpg)

The reason that getting to the number of people fully vaccinated was more complex is that the column "total of people fully vaccinated" was cumulative (every day it summed up the new people vaccinated). So to get to the total considering all the countries, I had to get the value in the last date (which is 19-May-2022). However, there were countries that had no vaccinations on this date, so in the measure, the result was coming wrongly as zero. The solution that I came up with after some thinking and search was the below:

![Medida](https://user-images.githubusercontent.com/105753824/171750016-4292b09d-b191-44b5-80c2-0eb8ac255a25.jpg)
 
## Dashboard
As we had clear what questions to answer since the beginning, there was not much to think about the indicators to be used, but I wanted to have everything on one single page, and add a Tooltip page. Hence, on the main page, there is information about the cases and deaths by country and continent, and a tooltip with the vaccination details per country was added.

![Dashboard](https://user-images.githubusercontent.com/105753824/171766151-77b7f96d-18aa-437e-b11a-29eb213f757c.jpg)

You can access the final version of this report in Power BI online by [clicking here](https://app.powerbi.com/view?r=eyJrIjoiMjA3M2U3NDktYzhmMi00M2JlLWJmY2YtZWYxOWM0MWExYWI5IiwidCI6IjBhMzUzNTNiLTUzZTUtNDI3Yy05YmJkLTU3Yzg3ZmEwZjZlNyJ9)

# Conclusion

By doing all the steps above, it was possible to get the answers to all the questions set at the beginning of this project. As of 19-May-2022, the total number of cases worldwide was 523M, and the total number of deaths 6M, making the mortality of COVID 1.19%, which is lower than I particularly thought. Also, 6.64% of the population got infected by covid. And currently, 4.7 billion have been fully vaccinated, which is almost 60% of the population. It is also possible to see that mortality has been decreasing considerably since the beginning of the pandemic. 

The country that had the highest number of cases is the United States, followed by India and Brazil. These 3 are also the countries that had the highest number of deaths. However, they are far away from being the countries with the highest mortality, whose top 3 has two countries in which the population fully vaccinated is below 8% (Sudan) and 1% percent (Yemen) - the third country is Peru, but it has 81.24% of its population vaccinated. The country that had the highest percentage of infection was Feroe Islands (70%), followed by Andorra (55%) and Gibraltar (54%). 

Regarding continents, Europe had the highest number of cases, deaths, and percentage of the population infected. Whereas South America had the highest mortality, with 2.23%, followed by Africa, with 2.15%. Oceania had the lowest mortality, with 0.14%. South America is also the continent that now has the highest percentage of the population vaccinated (75%). In this aspect, all the other continents seem to be on a similar pace (Asia (69%), Europe (66%), North America (64%), Oceania(63%)), except for Africa, which has only 17% of its population vaccinated, but for some reason had the lowest population infected, with 0.86% (one fact that could explain this is the unfortunate knowingly poor health structure in the continent, that might have resulted in not enough tests made, but this my personal opinion - in part 2 of this project (explained in the next paragraph) I will do this analysis).

With the questions that I proposed to answer, above are a few facts that I could take throughout this project. Working on this was really interesting (although sad because many lives were lost and many people were affected by the virus. My idea is to make it again in January/2023, but in an automated way through some API, so I can have the numbers up to date daily. In this next COVID project, I will also do further analysis of other indicators.
