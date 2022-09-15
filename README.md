# airbnbmalaga
Proyecto sobre viabilidad de negocios y posibilidad de contactos comerciales con los datos de AirBnB en Málaga.
Los datos los hemos obtenido de IndiseAirBnB(http://insideairbnb.com/get-the-data/)

Hemos utilizado los csv que se encuentran en este repositorio listings.csv y reviews.csv.gz

En este proyecto hemos buscado resolver dos preguntas principales según los datos que disponemos:
## 1) ¿Cuáles son las 50 propiedades con mayores ingresos mensuales?
Para responder esta pregunta hemos utilizado SQL, en concreto la siguiente query:

    SELECT id, listing_url, price, name, 30 - availability_30 AS reservados_30, 
    
    CAST(REPLACE(Price,'$','') AS UNSIGNED) AS price_num, 
    
    CAST(REPLACE(Price,'$','') AS UNSIGNED)*(30 - availability_30) / beds AS ingresos_30
    
    FROM listings 
    
    ORDER BY ingresos_30 DESC LIMIT 50; 
    
## 2) ¿A qué negocios podemos ofrecer un servicio de limpieza?
Para ello vamos a buscar los propietarios con un mayor número de reseñas que hacen referencia a sucio/sucia/dirty. Para ello hemos utilizado la siguiente query:
    
    SELECT host_id, host_url, host_id, host_name, comments, COUNT(host_name) AS num_sucio_dirty_reviews
    
    FROM reviews
    
    INNER JOIN listings ON reviews.listing_id = listing_id
    
    WHERE comments LIKE "%dirty%" OR "%suci%"
    
    GROUP BY host_id, host_url, host_name, comments
    
    HAVING COUNT(host_name) > 1
    
    ORDER BY num_sucio_dirty_reviews DESC;
