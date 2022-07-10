
/* 
Using COVID data from OurWorldInData.com from January 1, 2020 to July 5, 2022
*/

-- Create database and import data into world table
CREATE DATABASE covid;
USE covid;

-- WORLD MAP
/* I want to build 4 maps: Total Cases, Total Deaths, % Population Infected, and Vaccination Rates for each country. 
Not every sountry updates their records daily, so I pulled the max for these numbers for each country to use the most up to date number
since no one date will give me data for *every* country
*/
SELECT 
    iso_code AS 'ISO Code',
    continent AS 'Continent',
    location AS 'Location',
    MAX(population) AS 'Population',
    MAX(total_cases) AS 'Total Cases',
    MAX(CAST(total_deaths AS UNSIGNED)) AS 'Total Deaths',
    MAX(CAST(total_vaccinations AS UNSIGNED)) AS 'Total Vaccinations',
    MAX(CAST(people_fully_vaccinated AS UNSIGNED)) AS 'Fully Vaccinated People',
    MAX(CAST(people_fully_vaccinated_per_hundred AS UNSIGNED)) AS 'Vaccinated People per Hundred',
    MAX(CAST(total_vaccinations_per_hundred AS UNSIGNED)) AS 'Vaccinations per Hundred'
FROM
    world
GROUP BY location
ORDER BY location;


-- OVERALL TIMELINE
/*
I will use the daily data for the world, regions, and select countries.
I'm looking at new and cumulative cases/deaths/vaccines.
The regions are North America, South America, Asia, Oceania, Africa, and Europe.
*/
SELECT 
    iso_code AS 'ISO Code',
    location AS 'Location',
    continent AS 'Continent',
    date AS 'Date',
    new_cases AS 'New Cases',
    total_cases AS 'Total Cases',
    new_deaths AS 'New Deaths',
    total_deaths 'Total Deaths',
    new_vaccinations AS 'New Vaccinations',
    total_vaccinations AS 'Total Vaccinations',
    people_fully_vaccinated AS 'People Fully Vaccinated',
    people_fully_vaccinated_per_hundred AS 'People Fully Vaccinated per Hundred',
    total_vaccinations_per_hundred AS 'Total Vaccinations per Hundred'
FROM
    world
WHERE
    location IN ('Africa' , 'Asia',
        'Europe',
        'North America',
        'Oceania',
        'South America',
        'World'); 
	

-- United States
-- Just going to pull everything, just in case
SELECT 
    *
FROM
    world
WHERE
    location = 'United States';


/*
Will do a country breakdown of a few high profile nations
The countries I'm interested in are US, UK, Italy, Germany, France, Canada, Japan, India, and Australia
*/
SELECT 
    iso_code AS 'ISO Code',
    location AS 'Location',
    continent AS 'Continent',
    date AS 'Date',
    new_cases AS 'New Cases',
    total_cases AS 'Total Cases',
    new_deaths AS 'New Deaths',
    total_deaths 'Total Deaths',
    new_vaccinations AS 'New Vaccinations',
    total_vaccinations AS 'Total Vaccinations',
    people_fully_vaccinated AS 'People Fully Vaccinated',
    people_fully_vaccinated_per_hundred AS 'People Fully Vaccinated per Hundred',
    total_vaccinations_per_hundred AS 'Total Vaccinations per Hundred'
FROM
    world
WHERE
    location IN ('United States' , 'Canada',
        'France',
        'Germany',
        'Italy',
        'Japan',
        'United Kingdom',
        'Australia',
        'India'); 

-- -----------------------------------------------------------------------------------------------
-- Soem extra queries for fun (or, as fun as you can get talking about COVID)

-- Death Percentage - estimate of how likely average citizen is to die if they contract COVID
SELECT 
    location,
    date,
    total_cases,
    total_deaths,
    (total_deaths / total_cases) * 100 AS 'Death Percentage'
FROM
    world
WHERE
    iso_code LIKE '___'
ORDER BY 1 , 2;

-- Running tally of percentage of US population has gotten COVID (reported cases only)
SELECT 
    location,
    date,
    total_cases,
    population,
    (total_cases / population) * 100 AS 'Case Percentage'
FROM
    world
WHERE
    location = 'United States'
ORDER BY 1 , 2;

-- Highest Infection Rate compared to Population, ranked highest to lowest
SELECT 
    location,
    population,
    MAX(CAST(total_cases AS UNSIGNED)) AS 'Max # of Cases',
    MAX((total_cases / population)) * 100 AS 'Highest Infection Rate'
FROM
    world
GROUP BY location
ORDER BY 4 DESC;

-- Countries with the highest death count per population
SELECT 
    location,
    population,
    MAX(cast(total_deaths as unsigned)) AS 'Max # of Deaths',
    MAX(total_deaths / population) *100 AS 'Percent of Population Died'
FROM
    world
WHERE
    iso_code LIKE '___'
GROUP BY location
ORDER BY 4 DESC;


-- Global Vaccinations
-- Second doses and boosters all count as vaccine applications, so vaccines per person is a useful estimate 
-- (Would expect 2 per person to be a good benchmark for total population vaccination)
SELECT 
    date,
    SUM(cast(new_vaccinations as unsigned)) AS 'Daily New Vaccines',
	SUM(SUM(cast(new_vaccinations as unsigned))) OVER (ORDER BY date) AS 'Total Cumulative Vaccines',
    (SUM(SUM(cast(new_vaccinations as unsigned))) OVER (ORDER BY date)) / SUM(population) AS 'Vaccines Administered Per Person',
    SUM(SUM(cast(people_fully_vaccinated as unsigned))) OVER (ORDER BY date) AS 'People Fully Vaccinated'
FROM
    world
WHERE
    iso_code LIKE '___'
GROUP BY date
ORDER BY date;


-- THe GDP Per Capita of each country and the highest number of people in that country who are fully vaccinated
SELECT location, gdp_per_capita AS 'GDP Per Capita', 
MAX(cast(people_fully_vaccinated as unsigned)) AS 'Max Number of Fully Vaccinated People',
MAX(cast(people_fully_vaccinated as unsigned)) / population * 100 AS 'Highest % of Population Vaccinated'
FROM world
GROUP BY location
ORDER BY 4 DESC;


