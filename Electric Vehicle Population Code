CREATE TABLE ElectricVehicles (
    VIN VARCHAR(10),
    County VARCHAR(255),
    City VARCHAR(255),
    State VARCHAR(255),
    Postal_Code VARCHAR(10),  -- Assuming 10-digit zip code
    Model_Year INT,
    Make VARCHAR(255),
	Model Varchar(200),
    Electric_Vehicle_Type VARCHAR(200),
    CAFV_Eligibility VARCHAR(255),
    Electric_Range INT,
    Base_MSRP DECIMAL(15, 2),
    Legislative_District INT,
    DOL_Vehicle_ID INT,
    Vehicle_Location VARCHAR(255),
    Electric_Utility VARCHAR(255),
    Census_Tract_2020 VARCHAR(255)  -- Assuming alphanumeric, adjust if needed
);

Select * from ElectricVehicles;

--EASY LEVEL

/*Q1. Write a query to analyze the trend in the count of electric vehicles over the years, 
showing the count for each year.*/

SELECT Model_Year, COUNT(*) AS VehicleCount
FROM ElectricVehicles
GROUP BY Model_Year
ORDER BY Model_Year;

/*Q2.Identify the top 3 electric vehicle models with the highest base Manufacturer's 
Suggested Retail Price (MSRP).*/

SELECT Make, Model, Base_MSRP
FROM ElectricVehicles
ORDER BY Base_MSRP DESC
LIMIT 3;

/*Q3. Identify the top 5 counties with the highest number of registered electric vehicles.*/

SELECT County, COUNT(*) AS VehicleCount
FROM ElectricVehicles
GROUP BY County
ORDER BY VehicleCount DESC
LIMIT 5;

/*Q4. Determine the total count of electric vehicles registered in Washington state.*/

SELECT COUNT(*) AS VehicleCount
FROM ElectricVehicles
WHERE State = 'WA';

--MODRATE LEVEL

/*Q5.Calculate the deviation of each electric vehicle's range from the average
electric range of its corresponding model year.*/


SELECT VIN, Model_Year, Electric_Range,
    Electric_Range - AVG(Electric_Range) OVER (PARTITION BY Model_Year) AS RangeDeviation
FROM ElectricVehicles;

/*Q6. Provide a breakdown of the distribution of electric vehicle types (e.g., BEV, PHEV) 
for each county. Additionally,identify the county with the highest count of each electric 
vehicle type.*/

WITH EVTypeDistribution AS (
    SELECT 
        County,
        Electric_Vehicle_Type,
        COUNT(*) AS EVTypeCount,
        RANK() OVER (PARTITION BY Electric_Vehicle_Type ORDER BY COUNT(*) DESC) AS RankByTypeCount
    FROM ElectricVehicles
    GROUP BY County, Electric_Vehicle_Type
)
SELECT 
    County,
    Electric_Vehicle_Type,
    EVTypeCount
FROM EVTypeDistribution
WHERE RankByTypeCount = 1;  -- Select the highest count for each electric vehicle type

/*Q7. Compare the percentage of Clean Alternative Fuel Vehicle (CAFV) eligible electric vehicles 
among different makes. Identify the top 5 makes with the highest CAFV eligibility rates.*/

WITH MakeCAFVRate AS (
    SELECT 
        Make,
        COUNT(*) AS TotalEVs,
        SUM(CASE WHEN CAFV_Eligibility = 'Clean Alternative Fuel Vehicle' THEN 1 ELSE 0 END) AS CAFVEligibleCount,
        (SUM(CASE WHEN CAFV_Eligibility = 'Clean Alternative Fuel Vehicle' THEN 1 ELSE 0 END) * 100.0) / COUNT(*) AS CAFVRate
    FROM ElectricVehicles
    GROUP BY Make
)
SELECT 
    Make,
    TotalEVs,
    CAFVEligibleCount,
    CAFVRate
FROM MakeCAFVRate
ORDER BY CAFVRate DESC
LIMIT 5;

--HARD LEVEL

/*Q8.Find legislative districts where there is a diverse range of electric vehicle types,
considering both Battery Electric Vehicles (BEV) and Plug-in Hybrid Electric Vehicles (PHEV)*/

SELECT Make,Legislative_District, COUNT(DISTINCT Electric_Vehicle_Type) AS UniqueEVTypes
FROM ElectricVehicles
GROUP BY Legislative_District,Make
HAVING COUNT(DISTINCT Electric_Vehicle_Type) > 1
LIMIT 10;

/*Q9. Find VINs with irregularities in their structure, considering the first 10 characters.
This could include duplicates or unexpected patterns.*/

SELECT VIN, COUNT(*) AS DuplicateCount
FROM ElectricVehicles
GROUP BY VIN
HAVING COUNT(*) > 1;

/*Q10. Find electric vehicles in regions where the adoption of Clean Alternative Fuel Vehicles 
(CAFVs) is high, considering both the number of CAFVs and the total number of electric vehicles.*/

WITH CAFVRegion AS (
    SELECT County, COUNT(*) AS CAFVCount
    FROM ElectricVehicles
    WHERE CAFV_Eligibility = 'Clean Alternative Fuel Vehicle'
    GROUP BY County
)
SELECT ev.make, ev.County, COUNT(ev.VIN) AS TotalEVs, cr.CAFVCount
FROM ElectricVehicles ev
LEFT JOIN CAFVRegion cr ON ev.County = cr.County
GROUP BY ev.County, cr.CAFVCount, ev.make
ORDER BY cr.CAFVCount DESC;

Select distinct model from electricvehicles;


