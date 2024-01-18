**BUSINESS QUESTIONS**
* _1.Which movies contributed the most/least to sales?_
```
-- Generating a table with a movie list, rating, category, actor and total sale
-- Selecting movie-related information and total sales
SELECT
    A.title AS movie_title,                
    A.rating AS movie_rating,              
    F.name AS movie_category,              
    SUM(amount) AS total_sale             
FROM film A 
LEFT JOIN inventory B ON A.film_id = B.film_id       -- Joining with the inventory table to link films and inventory
LEFT JOIN rental C ON B.inventory_id = C.inventory_id -- Joining with the rental table to link inventory and rentals
LEFT JOIN payment D ON C.rental_id = D.rental_id     -- Joining with the payment table to link rentals and payments
LEFT JOIN film_category E ON A.film_id = E.film_id   -- Joining with the film_category table to link films and categories
LEFT JOIN category F ON E.category_id = F.category_id -- Joining with the category table to get category names
GROUP BY title, rating, F.name            
ORDER BY 4 DESC NULLS LAST;                  
```
```
-- List of actors and their total sales
SELECT
    H.actor_id,
    H.first_name AS actor_first_name,
    H.last_name AS actor_last_name,
    CONCAT(H.first_name, ' ', H.last_name) AS actor_full_name,
    SUM(D.amount) AS total_sales
FROM actor H
LEFT JOIN film_actor GA ON H.actor_id = GA.actor_id        -- Joining with the film_actor table to link actors and films
LEFT JOIN film A ON GA.film_id = A.film_id                 -- Joining with the film table to link film_actor and films
LEFT JOIN inventory B ON A.film_id = B.film_id             -- Joining with the inventory table to link films and inventory
LEFT JOIN rental C ON B.inventory_id = C.inventory_id     -- Joining with the rental table to link inventory and rentals
LEFT JOIN payment D ON C.rental_id = D.rental_id           -- Joining with the payment table to link rentals and payments
GROUP BY H.actor_id, actor_full_name                        -- Grouping the results by actor_id and actor_full_name
ORDER BY 5 DESC NULLS LAST;                                 -- Ordering the results by total sales in descending order
```

* _2. What was the average rental duration for all videos?_
```
SELECT
    MIN(rental_duration) AS min_rental_duration, -- 2 Businness question
    MAX(rental_duration) AS max_rental_duration, -- 2 Businness question
    AVG(rental_duration) AS avg_rental_duration -- 2 Businness question
FROM
    film;
```
* _3.Which countries are Rockbuster customers based in?_
```
_SELECT country,
       COUNT(DISTINCT A.customer_id) AS customer_count,
       SUM(amount) AS total_payment
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_ID = D.country_ID
INNER JOIN payment E ON a.customer_id = E.customer_id
GROUP BY country
ORDER BY 3 DESC
```
* _4. Where are customers with a high lifetime value based?_
```
-- Creating a Common Table Expression (CTE) to get the top ten cities within the top ten countries
WITH TopCitiesCTE AS (
    SELECT
        D.country,
        C.city,
        COUNT(A.customer_id) AS customer_count
    FROM
        customer A
        INNER JOIN address B ON A.address_id = B.address_id
        INNER JOIN city C ON B.city_id = C.city_id
        INNER JOIN country D ON C.country_id = D.country_id
-- Filtering for countries in the top 10 by customer count using a subquery
    WHERE
        D.country IN (
            SELECT D.country
            FROM customer A
            INNER JOIN address B ON A.address_id = B.address_id
            INNER JOIN city C ON B.city_id = C.city_id
            INNER JOIN country D ON C.country_id = D.country_id
            GROUP BY D.country
            ORDER BY COUNT(A.customer_id) DESC
            LIMIT 10
        )
    GROUP BY D.country, C.city
    ORDER BY customer_count DESC
    LIMIT 10
)

-- Main query using the CTE to filter by the top ten cities within the top ten countries
SELECT
    A.customer_id,
    A.first_name,
    A.last_name,
    D.country,
    C.city,
    SUM(E.amount) AS total_amount_paid
FROM
    customer A
    INNER JOIN payment E ON A.customer_id = E.customer_id
    INNER JOIN address B ON A.address_id = B.address_id
    INNER JOIN city C ON B.city_id = C.city_id
    INNER JOIN country D ON D.country_id = C.country_id
WHERE
    (D.country, C.city) IN (
        SELECT country, city FROM TopCitiesCTE
    )
GROUP BY
    A.customer_id,
    A.first_name,
    A.last_name,
    D.country,
    C.city
ORDER BY
    total_amount_paid DESC
LIMIT 5;
```
* _5. Do sales figures vary between geographic regions?_
```
--Sales by geographic regions as continents (ChatGPT 3.5 prompted)
--Create a continent table
CREATE TABLE continent_mapping (
    country VARCHAR(255) PRIMARY KEY,
    continent VARCHAR(255)
);
```
```
INSERT INTO continent_mapping (country, continent) VALUES
('India', 'Asia'),
('China', 'Asia'),
('United States', 'North America'),
('Japan', 'Asia'),
('Mexico', 'North America'),
('Brazil', 'South America'),
('Russian Federation', 'Europe'),
('Philippines', 'Asia'),
('Turkey', 'Asia'),
('Indonesia', 'Asia'),
('Nigeria', 'Africa'),
('Argentina', 'South America'),
('Taiwan', 'Asia'),
('South Africa', 'Africa'),
('Iran', 'Asia'),
('United Kingdom', 'Europe'),
('Poland', 'Europe'),
('Italy', 'Europe'),
('Germany', 'Europe'),
('Vietnam', 'Asia'),
('Ukraine', 'Europe'),
('Colombia', 'South America'),
('Egypt', 'Africa'),
('Venezuela', 'South America'),
('Canada', 'North America'),
('Netherlands', 'Europe'),
('South Korea', 'Asia'),
('Spain', 'Europe'),
('Yemen', 'Asia'),
('Pakistan', 'Asia'),
('Saudi Arabia', 'Asia'),
('Peru', 'South America'),
('Thailand', 'Asia'),
('Israel', 'Asia'),
('Ecuador', 'South America'),
('Bangladesh', 'Asia'),
('Algeria', 'Africa'),
('France', 'Europe'),
('Malaysia', 'Asia'),
('Tanzania', 'Africa'),
('Mozambique', 'Africa'),
('United Arab Emirates', 'Asia'),
('Dominican Republic', 'North America'),
('Chile', 'South America'),
('Austria', 'Europe'),
('Morocco', 'Africa'),
('Paraguay', 'South America'),
('Belarus', 'Europe'),
('Latvia', 'Europe'),
('Switzerland', 'Europe'),
('Kenya', 'Africa'),
('Yugoslavia', 'Europe'), -- Note: Yugoslavia is no longer a country; included for the provided list
('Puerto Rico', 'North America'),
('Romania', 'Europe'),
('Runion', 'Africa'), -- Note: Assuming this refers to Réunion island, included for the provided list
('French Polynesia', 'Oceania'),
('Greece', 'Europe'),
('Sudan', 'Africa'),
('Azerbaijan', 'Asia'),
('Bulgaria', 'Europe'),
('Kazakhstan', 'Asia'),
('Angola', 'Africa'),
('Cameroon', 'Africa'),
('Myanmar', 'Asia'),
('Cambodia', 'Asia'),
('Bolivia', 'South America'),
('Congo, The Democratic Republic of the', 'Africa'),
('Oman', 'Asia'),
('Holy See (Vatican City State)', 'Europe'),
('Nauru', 'Oceania'),
('Sweden', 'Europe'),
('Czech Republic', 'Europe'),
('Moldova', 'Europe'),
('Turkmenistan', 'Asia'),
('Chad', 'Africa'),
('Malawi', 'Africa'),
('Zambia', 'Africa'),
('Virgin Islands, U.S.', 'North America'),
('Greenland', 'North America'),
('Armenia', 'Asia'),
('Gambia', 'Africa'),
('Iraq', 'Asia'),
('Hungary', 'Europe'),
('Bahrain', 'Asia'),
('North Korea', 'Asia'),
('Brunei', 'Asia'),
('Kuwait', 'Asia'),
('Estonia', 'Europe'),
('Hong Kong', 'Asia'),
('Sri Lanka', 'Asia'),
('Liechtenstein', 'Europe'),
('Anguilla', 'North America'),
('French Guiana', 'South America'),
('Faroe Islands', 'Europe'),
('Senegal', 'Africa'),
('Nepal', 'Asia'),
('Tuvalu', 'Oceania'),
('Madagascar', 'Africa'),
('Ethiopia', 'Africa'),
('New Zealand', 'Oceania'),
('Slovakia', 'Europe'),
('Finland', 'Europe'),
('Tunisia', 'Africa'),
('Afghanistan', 'Asia'),
('Tonga', 'Oceania'),
('Saint Vincent and the Grenadines', 'North America'),
('Lithuania', 'Europe'),
('American Samoa', 'Oceania');
```
```
UPDATE continent_mapping
SET country = 'Kazakstan'
WHERE country = 'Kazakhstan';
```
```
--Adding the continents to the sales per country results (ChatGPT 3.5 prompted)
SELECT 
    D.country,
    COALESCE(F.continent, 'Unknown') AS continent,
    COUNT(DISTINCT A.customer_id) AS customer_count,
    SUM(amount) AS total_payment
FROM customer A
INNER JOIN address B ON A.address_id = B.address_id
INNER JOIN city C ON B.city_id = C.city_id
INNER JOIN country D ON C.country_ID = D.country_ID
INNER JOIN payment E ON A.customer_id = E.customer_id
LEFT JOIN continent_mapping F ON LOWER(D.country) = LOWER(F.country)
GROUP BY D.country, F.continent
ORDER BY 3 DESC;
```
```
--Favourite genre per continent (ChatGPT 3.5 prompted)
-- Common Table Expression (CTE) to calculate rental counts for each film category in each continent
WITH ContinentFilmCounts AS (
    SELECT 
        COALESCE(CM.continent, 'Unknown') AS continent,  -- Use continent from continent_mapping; default to 'Unknown' if not available
        COALESCE(C.category_id, 0) AS category_id,  -- Use category_id from category; default to 0 if not available
        R.rental_id,
        COUNT(DISTINCT R.rental_id) AS rental_count  -- Count distinct rental_ids for each category in each continent
    FROM rental R
    INNER JOIN inventory I ON R.inventory_id = I.inventory_id
    INNER JOIN film_category FC ON I.film_id = FC.film_id
    INNER JOIN category C ON FC.category_id = C.category_id
    INNER JOIN customer CUST ON R.customer_id = CUST.customer_id
    INNER JOIN address A ON CUST.address_id = A.address_id
    INNER JOIN city CTY ON A.city_id = CTY.city_id
    INNER JOIN country CNTRY ON CTY.country_ID = CNTRY.country_ID
    LEFT JOIN continent_mapping CM ON CNTRY.country = CM.country
    GROUP BY CM.continent, C.category_id, R.rental_id
)

-- Select columns from the ContinentFilmCounts CTE and join with the category table
-- This query retrieves the total rentals and total payment for each film category in each continent
-- Results are grouped by continent, category_name, and ordered by total_rentals in descending order
SELECT 
    CF.continent,
    COALESCE(C.name, 'Unknown') AS category_name,
    COUNT(DISTINCT CF.rental_id) AS total_rentals,
    SUM(P.amount) AS total_payment
FROM ContinentFilmCounts CF
LEFT JOIN category C ON CF.category_id = C.category_id
LEFT JOIN rental R ON CF.rental_id = R.rental_id
LEFT JOIN payment P ON R.rental_id = P.rental_id
GROUP BY CF.continent, C.name
ORDER BY CF.continent, total_rentals DESC;
```
