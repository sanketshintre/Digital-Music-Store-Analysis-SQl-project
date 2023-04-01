# Digital-Music-Store-Analysis-SQl-project

# Objective - Analyze and Examine the Dataset with SQL and help the Store understand its Business growth by answering simple questions.

/* Q1: Who is the senior most employee based on job title? 
/* Q1: Who is the senior most employee based on job title? 
/* Q2: Which countries have the most Invoices? 
/* Q3: What are top 3 values of total invoice?
/* Q4: Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. 
Write a query that returns one city that has the highest sum of invoice totals. 
/* Q5: Who is the best customer? The customer who has spent the most money will be declared the best customer. 
Write a query that returns the person who has spent the most money.
Return both the city name & sum of all invoice total?
/* Q6: Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
Return your list ordered alphabetically by email starting with A?
/* Q7: Let's invite the artists who have written the most rock music in our dataset?
Write a query that returns the Artist name and total track count of the top 10 rock bands?


SELECT title, last_name, first_name 
FROM employee
ORDER BY levels DESC
LIMIT 1

<img width="960" alt="2023-02-25 (1)" src="https://user-images.githubusercontent.com/123626990/229287730-36d44852-5460-4d9d-aa8c-fecf8c60acc3.png">


/* Q2: Which countries have the most Invoices? */

SELECT COUNT(*) AS c, billing_country 
FROM invoice
GROUP BY billing_country
ORDER BY c DESC

<img width="960" alt="2023-02-25 (2)" src="https://user-images.githubusercontent.com/123626990/229287748-d9c867c7-0e42-485b-8c2f-881e400fab6a.png">


/* Q3: What are top 3 values of total invoice? */

SELECT total 
FROM invoice
ORDER BY total DESC

<img width="960" alt="2023-02-25 (3)" src="https://user-images.githubusercontent.com/123626990/229287757-c5439360-d903-4728-a497-0fdd8d0a6aa6.png">


/* Q4: Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. 
Write a query that returns one city that has the highest sum of invoice totals. 
Return both the city name & sum of all invoice totals */

SELECT billing_city,SUM(total) AS InvoiceTotal
FROM invoice
GROUP BY billing_city
ORDER BY InvoiceTotal DESC
LIMIT 1;

<img width="960" alt="2023-02-25 (4)" src="https://user-images.githubusercontent.com/123626990/229287812-ceef132b-fd1b-45b0-bc1c-b0de160aa708.png">


/* Q5: Who is the best customer? The customer who has spent the most money will be declared the best customer. 
Write a query that returns the person who has spent the most money.*/

SELECT customer.customer_id, first_name, last_name, SUM(total) AS total_spending
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
GROUP BY customer.customer_id
ORDER BY total_spending DESC
LIMIT 1;

<img width="960" alt="2023-02-25 (5)" src="https://user-images.githubusercontent.com/123626990/229287831-7826a3c1-e55c-4a2d-8a9e-1be9304328dd.png">

/* Q6: Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
Return your list ordered alphabetically by email starting with A. */


SELECT DISTINCT email,first_name, last_name
FROM customer
JOIN invoice ON customer.customer_id = invoice.customer_id
JOIN invoiceline ON invoice.invoice_id = invoiceline.invoice_id
WHERE track_id IN(
	SELECT track_id FROM track
	JOIN genre ON track.genre_id = genre.genre_id
	WHERE genre.name LIKE 'Rock'
)
ORDER BY email;

<img width="960" alt="2023-02-25 (6)" src="https://user-images.githubusercontent.com/123626990/229287841-1f91408e-0968-47f3-a3b0-3bcab104b798.png">


/* Q7: Let's invite the artists who have written the most rock music in our dataset. 
Write a query that returns the Artist name and total track count of the top 10 rock bands. */

SELECT artist.artist_id, artist.name,COUNT(artist.artist_id) AS number_of_songs
FROM track
JOIN album ON album.album_id = track.album_id
JOIN artist ON artist.artist_id = album.artist_id
JOIN genre ON genre.genre_id = track.genre_id
WHERE genre.name LIKE 'Rock'
GROUP BY artist.artist_id
ORDER BY number_of_songs DESC
LIMIT 10;

<img width="960" alt="2023-02-25 (7)" src="https://user-images.githubusercontent.com/123626990/229287859-8aa2d3e5-6817-4615-8767-7ee42546d1b9.png">



/* Q8: Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first. */

SELECT name,miliseconds
FROM track
WHERE miliseconds > (
	SELECT AVG(miliseconds) AS avg_track_length
	FROM track )
ORDER BY miliseconds DESC;

<img width="960" alt="2023-02-25 (8)" src="https://user-images.githubusercontent.com/123626990/229287937-813ea58f-3495-410b-b4ae-f19ff2db182b.png">


