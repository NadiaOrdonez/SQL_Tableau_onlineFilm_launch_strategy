--DATA OVERVIEW
--Film table statistical description
SELECT
    COUNT (film_id) AS total_movies,
	MIN(release_year) AS min_release_year,
    MAX(release_year) AS max_release_year,
    AVG(release_year) AS avg_release_year,
    COUNT(DISTINCT language_id) AS total_languages,
    MIN(rental_duration) AS min_rental_duration, -- 2 Businness question
    MAX(rental_duration) AS max_rental_duration, -- 2 Businness question
    AVG(rental_duration) AS avg_rental_duration, -- 2 Businness question
    MIN(rental_rate) AS min_rental_rate,
    MAX(rental_rate) AS max_rental_rate,
    AVG(rental_rate) AS avg_rental_rate,
    MIN(length) AS min_length,
    MAX(length) AS max_length,
    AVG(length) AS avg_length,
    MIN(replacement_cost) AS min_replacement_cost,
    MAX(replacement_cost) AS max_replacement_cost,
    AVG(replacement_cost) AS avg_replacement_cost,
    MODE() WITHIN GROUP (ORDER BY rating) AS mode_rating
FROM
    film;

--Which rating does the movies have?
SELECT DISTINCT rating,
		COUNT (rating) AS rating_count
FROM film
GROUP BY rating
ORDER BY 2 DESC

--Which language do the movies in the film table are?
SELECT DISTINCT A.language_id,
		COUNT (A.language_id) AS language_count,
		B.name As language_name
FROM film A
LEFT JOIN language B ON A.language_id = B.language_id
GROUP BY A.language_id,
		 B.name
ORDER BY 2 DESC

--How many film categories do we have and which one are those?
SELECT 	C.name As movie_category,
		COUNT (A.film_id) AS film_count
FROM film A
LEFT JOIN film_category B ON A.film_id = B.film_id
LEFT JOIN category C ON B.category_id = C.category_id
GROUP BY C.name
ORDER BY 2 DESC

--Customer table statistical description
SELECT
    COUNT (DISTINCT customer_id) AS total_customers,
    COUNT (DISTINCT store_id) AS total_stores,
    MODE() WITHIN GROUP (ORDER BY first_name) AS mode_first_name,
    MODE() WITHIN GROUP (ORDER BY last_name) AS mode_last_name,
    MODE() WITHIN GROUP (ORDER BY email) AS mode_email,
    COUNT (DISTINCT address_id) AS total_addresses,
    MODE() WITHIN GROUP (ORDER BY activebool) AS mode_activebool,
    MIN(create_date) AS min_create_date,
    MAX(create_date) AS max_create_date,
	TO_TIMESTAMP(AVG(EXTRACT(EPOCH FROM create_date))) AS avg_create_date,--AI suggestion
    MODE() WITHIN GROUP (ORDER BY last_update) AS mode_last_update,
    COUNT (DISTINCT active) AS status_type
FROM
    customer;
	
--How many customers are active?
SELECT
    DISTINCT active,
	COUNT (active)
FROM customer
GROUP BY active;

--In how many countries our customers are located?
SELECT COUNT(DISTINCT D.country_id) AS total_countries
FROM customer A
LEFT JOIN address B ON A.address_id = B.address_id
LEFT JOIN city C ON B.city_id = C.city_id
LEFT JOIN country D ON C.country_id = D.country_id;

--How much sales do we generate?
--From which time period and how many payments = sales do we have data from?
SELECT
    COUNT (payment_id) AS total_payments,
    MIN(payment_date) AS early_payment_date,
    MAX(payment_date) AS latest_payment_date --all payments were generated in 2007
FROM
    payment;

--How much sales do we generate in 2007 and how many sales per customer in avg?
WITH total_payment_per_customer AS (SELECT
	customer_id,
	SUM (amount) AS total_payment,
	COUNT(payment_id) AS payment_per_customer
FROM
    payment
GROUP BY 
	customer_id
ORDER BY 1 )

SELECT
 SUM(total_payment) AS all_sales,--total sales in 2007
 AVG(total_payment) AS avg_all_sales, --total sales per customer in 2007
 STDDEV_SAMP (total_payment) AS sd_all_sales,--SD sales per customer in 2007
 MAX(total_payment) AS max_sale, --MAX sales per customer in 2007
 MIN(total_payment) AS min_sale,--MIN sales per customer in 2007
 AVG(payment_per_customer) AS avg_payment_count_per_customer --avg payment counts per customer in 2007
FROM total_payment_per_customer

--How much total rentals do we have?
SELECT COUNT(rental_id)
FROM rental
