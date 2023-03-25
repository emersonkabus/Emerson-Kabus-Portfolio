# Data Analysis Portfolio
### Emerson Kabus

## COVID Data Exploration with SQL

 ### Here I am taking a look at some COVID-related data found at https://ourworldindata.org/covid-deaths
 
 [Check out the Tableau Dashboard I made using this data!](https://public.tableau.com/views/COVIDDashboard_16797769219990/Dashboard1?:language=en-US&publish=yes&:display_count=n&:origin=viz_share_link)

-- taking a look at the data
Select *
From [dbo].[CovidDeaths$] 
order by 3,4

-- Percentage chance of dying if you contract COVID
Select Location, total_cases,total_deaths, (total_deaths/total_cases)*100 as DeathPercentage
From [dbo].[CovidDeaths$]
--Where location like '%states%'
order by 1,2


--Countries with highest death count per population
Select Location, MAX(cast(Total_deaths as int)) as TotalDeathCount, MAX((total_deaths/population))*100 as HighestDeathPercentage
From [dbo].[CovidDeaths$]
Where continent is not null 
Group by Location
order by HighestDeathPercentage desc


--Global Death Percentage
Select SUM(new_cases) as total_cases, SUM(cast(new_deaths as int)) as total_deaths, SUM(cast(new_deaths as int))/SUM(New_Cases)*100 as DeathPercentage
From [dbo].[CovidDeaths$]
where continent is not null 
order by 1,2

Select Location, SUM(cast(new_deaths as INT)) as TotalDeathCount
From[dbo].[CovidDeaths$]
Where Continent is null 
and Location not in ('World', 'European Union', 'International')
Group by Location
Order by TotalDeathCount Desc

--Countries with highest percentage of population infected
Select Location, Population, MAX(total_cases) as HighestInfectionCount,  Max((total_cases/population))*100 as PercentPopulationInfected
From [dbo].[CovidDeaths$]
Group by Location, Population
order by PercentPopulationInfected desc

--Infection Percentage over time, great for a visualization
Select Location, Population, date, MAX(total_cases) as HighestInfectionCount,  Max((total_cases/population))*100 as PercentPopulationInfected
From [dbo].[CovidDeaths$]
Group by Location, Population, Date
order by PercentPopulationInfected desc

--Cumulative Vaccination Count
Select dea.continent, dea.location, dea.date, dea.population, vac.new_vaccinations
, SUM(CONVERT(int,vac.new_vaccinations)) OVER (Partition by dea.Location Order by dea.location, dea.Date) as RollingPeopleVaccinated
From [dbo].[CovidDeaths$] dea
Join [dbo].[CovidVaccinations$] vac
	On dea.location = vac.location
	and dea.date = vac.date
where dea.continent is not null 
order by 2,3

## Project 1 
#### Bike Share Analysis

This project was a part of the Capstone Case Study within the Google Data Analytics Professional Certificate. Cyclistic is a fictional bike-share company in Chicago with over 5,800 bicycles and 600 docking stations. The company offers two different pricing plans: single ride passes, full day passes, and annual memberships. Although this model helps attract more customers, the company believes that maximizing the amount of annual memberships is the best strategy for future growth. In order to convert more one-time users to members, the marketing team needs to know how one-time users and members differ in their riding habits. 

The data I used was historical trip data for the year of 2019. It was provided by Google as part of the course.

The goal of my analysis was to identify patterns in usage between one-time users and annual members within the trip data.

![](/duration_by_usertype.png)

Clearly, customers (one-timee users) use the bikes for much longer durations than subscribers (1hr6min/trip to 15min/trip). Discounts or rewards for longer trips could be a good way to convert more one-time users to subscribers. 


![](/rides_by_usertype.png)

Subscribers use the bikes much more during the weekdays and most likely use the service as a means to commute, while customer usage peaks during the weekend, likely for leisure. 


![](/rides_by_gender_R.png)

The data on the gender of customers is not of much use, as male vs. female ride counts are quite similar and considering the vast number of N/A'.s The subscriber data however, clearly shows that there are far more male subscribers than female, so that is a demographic that the marketing team should certainly target. 

See the full code I used [here.](https://github.com/emersonkabus/Google-Data-Analytics-Certificate-Capstone-Case-Study/blob/main/Capstone%20Script.R)


## Project 2
#### Global CO2 Emissions Tableau Dashboard

I used a public [dataset](https://www.kaggle.com/datasets/yoannboyere/co2-ghg-emissionsdata) that I found on Kaggle to create a dashboard that showcases historical CO2 emissions trends since 1900. [Check it out!](https://public.tableau.com/app/profile/emerson2768/viz/EmissionsWorkbook_16700970459730/Dashboard1)
