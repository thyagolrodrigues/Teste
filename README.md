# COVID Data Exploration with SQL and Power BI
	
Below I will walk you through each step of this project. I want to showcase my SQL and Power BI skills, as well as show my organization process, and logical thinking when it comes to data projects, so future employers may have an idea of how I work. 

If you want to see only the SQL and Power BI part, you can skip to the EDA (Exploratory Data Analysis).

# Reason of this project:
Currently (May 2022), COVID occurrences are not the same as a few months ago, so I was wondering what countries were impacted by the virus the most. There are several ways to find out that - news, online dashboards, etc. However, I want to get to this answers by manipulating and organizing data myself, so I can also include this in my portfolio for future employers to see.
The idea is to answer the below questions by the end of this project:
-	Total number of cases/deaths globally;
-	Population infected;
-	Mortality; and
-	Number of people vaccinated as well as percentage that it represents
It is important to highlight that there are several indicators that might be analyzed. However, the idea of this project is not to go in depth. I will just work on the above points.

# Choosing the database:
I chose the COVID-19 database that is available in the Our World in Data website, which is a serious source that I know and have used before for other analysis.

# Data extraction:
The database comes in the CSV format, but the idea is to use SQL. And to make things more interesting, I divided the main file in two different files, one with the cases/deaths data, and the other with the vaccination data. Hence, I must use more functions in SQL (LEFT JOIN, for instance). Lastly, I imported each file as a table in a SQL database that I created, named PortfolioProject_COVID.

# EDA (Exploratory Data Analysis):
The databases from Our World in Data comes well-organized, so this was not a project that had much cleansing/transformation, but there will others in my portfolio to showcase my ability to do that.
Below you can see the SQL query with the coding. I commented each step (initial look, understanding the columns, etc). I could answer all the questions only by using SQL, but I want to use Power BI for visualization. Hence, I did both, I left the code to answer each of the questions in the query, and later you will see I got to the same results in POWER BI.

Below is the table that I created the view to use in Power BI. However, if you want to see to see how I got to the below, please click on "Click here to see the whole code" located right after the code box below.

``` sql

CREATE VIEW DeathsAndVaccinations AS
SELECT
	death.continent,
	death.location,
	death.date,
	death.population,
	death.total_cases,
	death.new_cases,
	death.total_deaths,
	death.new_deaths,
	vaccinations.total_tests,
	vaccinations.new_tests,
	vaccinations.total_vaccinations,
	vaccinations.new_vaccinations,
	vaccinations.people_vaccinated,
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
	
-- Initial look at the complete Deaths table: see how it is set, see the columns, and the data:

SELECT
	*
FROM
	PortfolioProject_COVID..CovidDeaths

-- Initial look at the complete Deaths table: ordering by continent and country, for better visualization, also I could notice that continent has many lines as NULL, so let's investigate the reason

SELECT
	*
FROM
	PortfolioProject_COVID..CovidDeaths
ORDER BY
	continent ASC,
	location ASC
WHERE
	continent is NULL

-- I want to look at the distinct values of the column locatino when filed by continent NULL

SELECT DISTINCT
	location
FROM
	PortfolioProject_COVID..CovidDeaths
WHERE
	continent is NULL
ORDER BY
	location ASC


-- Initial look at the complete Deaths table: ordering by continent and country, and considering only the NOT NULL for the column continent, as I noticed that these lines have another classification (such as High Income, Low Income) instead of the locations themselves

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

-- Selecting the columns that I will bring to Power BI for the purpose that we have. As this query results 176.870 mil linhas, I will not add the calculations here, I believe that DAX in Power BI will perform better than adding 176.870 for each column to be added. Also, I will use LEFT JOIN to get the columns that I will need from the table Vaccinations.

-- This is the same query from the beginning. However, after this one, I will perform that queries that would bring the results from Power BI

SELECT
	death.continent,
	death.location,
	death.date,
	death.population,
	death.total_cases,
	death.new_cases,
	death.total_deaths,
	death.new_deaths,
	vaccinations.total_tests,
	vaccinations.new_tests,
	vaccinations.total_vaccinations,
	vaccinations.new_vaccinations,
	vaccinations.people_vaccinated,
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
    

# Power BI (Data Model, DAX, and Dashboard):

# - Data Model
As there is only one table, the only link that there will be is between the COVID-19 database and the Calendar table (created by me in Power BI). I normally create a new table to add all the measures created, I think it is better to organize this way. 

 ![Model](https://user-images.githubusercontent.com/105753824/171749957-cedf60f4-7f6a-4e10-af79-1886e81f0a53.jpg)

# - DAX
Creation of the calendar table, and some measures for the graphics. The measures were relatively simple. The only one that more challenge was the one to define the quantity of people fully vaccinated… EXPLAIN WHY:

![Medidas](https://user-images.githubusercontent.com/105753824/171749997-631780fe-35c4-4fda-b575-8fa8ed656638.jpg)

![Medida](https://user-images.githubusercontent.com/105753824/171750016-4292b09d-b191-44b5-80c2-0eb8ac255a25.jpg)
 
# - Dashboard
As we had clear what questions to answer since the beginning, there was not much to think about the indicators to be used, but I wanted to use different graphics, with colours that make sense with COVID bla bla bla. With only one page with the results in a visual way. In the main page there the cases and vaccination graphics, and I added as Tooltip the vaccination details per country.

You can see the dashboard on this link:

https://app.powerbi.com/view?r=eyJrIjoiMjA3M2U3NDktYzhmMi00M2JlLWJmY2YtZWYxOWM0MWExYWI5IiwidCI6IjBhMzUzNTNiLTUzZTUtNDI3Yy05YmJkLTU3Yzg3ZmEwZjZlNyJ9

And below you can see a screenshot using a filter and the Tooltip:

![Dashboard](https://user-images.githubusercontent.com/105753824/171766151-77b7f96d-18aa-437e-b11a-29eb213f757c.jpg)

# Conclusion

Through the steps above, it was possible to get the answers to all the questions set at the beginning of this project. 

As of 19-May-2022, the total number of cases worldwide was 523M, and 6M deaths, making the mortality of covid 1.19%, which is lower than I particulaty thought initially. 6.64% of the population got covid. And currently 4.7 billion have been fully vaccinated, that is almost 60% of the population. It is also possible to see that the mortality has been decresing considerably since the beginning of the pandemic. 

The country that had the most quantity of cases is United States, followed by India and Brazil. These 3 are also the countries that had the most quantity of deaths. However, they are far away from being the countries with the worst mortality, whose top 3 has two countries which population fully vaccinated is below 8% (Sudan) and 1% percent (Yemen) - the third country is Peru, but it has 81.24% of its population vaccinated. The country had the highest percentage of infection was Feroe slands (70%), followed by Andorra (55%) and Gilbratar (54%). 

Speaking of continents, Europe had the highest number of cases, deaths, and percentage of population infected. Whereas South America had the worst mortality, with 2.23%, and Oceania the best, with 0.14%. South America is also the continent that now has highest percentage of population vaccinated (75%). When it comes to population fully vaccinated, all the continents seem to be on a similar pace (Asia (69%), Europe (66%), North America (64%), Oceania(63%)), except for Africa, which has only 17% of its population vaccinated, but for some reason had the lowest population infected, with 0.86%.

With the the answers that we planned to answer, above is a few facts that I could take throughout this project. This project was really interesting and I my idea is to re-do it in one year, but in an automated way through some API, so I can have the numbers up to date. In this next COVID project I will go in more depth and make more analysis, such as 
