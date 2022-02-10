# Chrisssh.github.io
Description of interests, projects and works

## Description
I am a CS graduate student from University of Pennsylvania, interested in machine-learning, data science, and softare development in general. I will graduate on May 2022. My undergraduate major was Computer Science and Mathematics.
## Interests
With a solid understanding of data structures and practice in solving algorithmic problems, I am very proficient in software development in Java, Python or TypeScript. I excel at Software Foundations, Theory and Algorithms. 
Additionally, I have passion for learning about topics in Deep Learning, Vision, Data Science, Big Data Analytics and Databases. I have also enjoyed my experience in full-stack development, using various frontend and backend frameworks, e.g. React, Angular, MongoDB, MySQL and NodeJS. 

## Projects
1. TraNYCparency: A web app for safe AirBnB listings using NYC crime data. My main contributions are implementing ten complex SQL quries and MongoDB data models and optimizing the queries' running time.
[Full report](https://drive.google.com/file/d/1MfZsNIIK02BHtzqyVQ24_6Z0QsXFbLu9/view?usp=sharing).

**Example Schema Design**
```
Airbnbs(airbnb_id, airbnb_name, room_type, minimum_nights, price, availability_365, image_path, latitude, longitude)
Reviews(airbnb_id, review_number, reviews_per_month, last_review_time)
Locations(latitude, longitude, zip_code)
NYPD_complaints(complaint_id, latitude, longitude, offense_level, date)
Wishlists(username, airbnb_id) Subway(station_id, station_name,
latitude, longitude, line); User(username, password)
```
**Example query**
```
const sharedQuery =
  'WITH NYPD_zipcode AS( ' +
  'SELECT N.latitude, N.longitude, L.zip_code, N.offense_level ' +
  'FROM NYPD_complaints N JOIN Locations L ' +
  'ON N.latitude = L.latitude AND N.longitude = L.longitude), ' +
  'AB_zipcode AS( ' +
  'SELECT A.airbnb_id, A.room_type, A.minimum_nights, A.price, L.zip_code, L.latitude, L.longitude ' +
  'FROM Airbnbs A LEFT JOIN Locations L ' +
  'ON A.latitude = L.latitude AND A.longitude = L.longitude), ' +
  'M AS (SELECT zip_code, 2 * COUNT(*) AS num ' +
  'FROM NYPD_zipcode ' +
  "WHERE offense_level = 'MISDEMEANOR' " +
  'GROUP BY zip_code), ' +
  'F AS (SELECT zip_code, 5 * COUNT(*) AS num ' +
  'FROM NYPD_zipcode ' +
  "WHERE offense_level = 'FELONY' " +
  'GROUP BY zip_code), ' +
  'Crimes AS (SELECT zip_code, SUM(num) AS num ' +
  'FROM ((SELECT * FROM M) UNION ALL (SELECT * FROM F)) VMF ' +
  'GROUP BY zip_code), ' +
  'AB_list_150 AS( ' +
  'SELECT L.airbnb_id, L.zip_code, L.latitude, L.longitude, IFNULL(C.num, 0) AS crime_index ' +
  'FROM AB_zipcode L LEFT JOIN Crimes C ' +
  'ON L.zip_code = C.zip_code ' +
  'ORDER BY crime_index ' +
  'LIMIT 150), ' +
  'diff AS( ' +
  'SELECT A.airbnb_id, A.zip_code, A.crime_index, S.station_name, (ABS(S.latitude-A.latitude) + ABS(S.longitude-A.longitude)) AS sum_ll ' +
  'FROM AB_list_150 A, (SELECT Sw.station_name, Sw.latitude, Sw.longitude FROM Subway Sw) S), ' +
  'min_diff AS( ' +
  'SELECT D.airbnb_id, MIN(sum_ll) AS min_diff ' +
  'FROM diff D ' +
  'GROUP BY D.airbnb_id), ' +
  'safest AS( ' +
  'SELECT DISTINCT D.airbnb_id, D.zip_code, D.crime_index, D.station_name, D.sum_ll AS lat_long_diff ' +
  'FROM min_diff M JOIN diff D ' +
  'ON M.airbnb_id = D.airbnb_id ' +
  'WHERE M.min_diff = D.sum_ll ' +
  'ORDER BY D.crime_index) ' +
  'SELECT DISTINCT A.airbnb_id, airbnb_name, image_path, price, minimum_nights, room_type, L.zip_code, L.latitude, L.longitude, review_number, reviews_per_month, station_name ' +
  'FROM Airbnbs A, Locations L, Reviews R, safest ' +
  'WHERE A.latitude = L.latitude ' +
  'AND A.longitude = L.longitude ' +
  'AND A.airbnb_id = R.airbnb_id ' +
  'AND A.airbnb_id = safest.airbnb_id ';
```
2. Kaggle: Prediction Competition ASHRAE-Great Energy Predictor III
3. Algorithmic Foundations Research in Computational Biology
