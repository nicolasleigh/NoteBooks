Additional resources:

> [PostgreSQL Documentation](https://www.postgresql.org/docs/)
>
> [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)

```sh
# Pull course docker images
docker pull postgres:14
docker pull btholt/complete-intro-to-sql
```

```sh
# Run a container whose name is pg
docker run -e POSTGRES_PASSWORD=lol --name=pg --rm -d -p 5432:5432 postgres:14
```

```sh
# Use this command to see the meaning of the flag
docker run --help
```

```sh
# Use this command to show running containers
docker ps
```

```sh
# Kill the contianer which name is pg
docker kill pg
```

```sh
# Execute a bash command in the running container
# Username is postgres, container name is pg, command is psql
docker exec -u postgres -it pg psql

# psql has a bunch of built in commands, for example, type \l to list all databases, type \? to see help, type \d to list all tables
```

```sql
CREATE DATABASE recipeguru;
-- type \c recipeguru to connect new database (currently "postgres")
```

```sql
CREATE TABLE ingredients (
  id INTEGER PRIMARY KEY GENERATED ALWAYS AS IDENTITY,
  title VARCHAR ( 255 ) UNIQUE NOT NULL
);
```

GENERATED ALWAYS AS IDENTITY : [PostgreSQL Identity Column](https://www.postgresqltutorial.com/postgresql-tutorial/postgresql-identity-column/)

```sql
INSERT INTO ingredients (title) VALUES ('bell pepper');
```

```sql
SELECT * FROM ingredients;
```

```sql
ALTER TABLE ingredients ADD COLUMN image VARCHAR ( 255 );
```

```sql
ALTER TABLE ingredients DROP COLUMN image;
```

```sql
ALTER TABLE ingredients
ADD COLUMN image VARCHAR ( 255 ),
ADD COLUMN type VARCHAR ( 50 ) NOT NULL DEFAULT 'vegetable';
```

```sql
INSERT INTO ingredients (
 title, image, type
) VALUES (
  'red pepper', 'red_pepper.jpg', 'vegetable'
);
```

```sql
INSERT INTO "ingredients" (
 "title", "image", "type" -- Notice the " here
) VALUES (
  'broccoli', 'broccoli.jpg', 'vegetable' -- and the ' here
);
```

The above query works because the double quotes are around identifiers like the table name and the column names. The single quotes are around the literal values. The double quotes above are optional. The single quotes are not.

```sql
INSERT INTO ingredients (
  title, image, type
) VALUES
  ( 'avocado', 'avocado.jpg', 'fruit' ),
  ( 'banana', 'banana.jpg', 'fruit' ),
  ( 'beef', 'beef.jpg', 'meat' ),
  ( 'black_pepper', 'black_pepper.jpg', 'other' ),
  ( 'blueberry', 'blueberry.jpg', 'fruit' ),
  ( 'broccoli', 'broccoli.jpg', 'vegetable' ),
  ( 'carrot', 'carrot.jpg', 'vegetable' ),
  ( 'cauliflower', 'cauliflower.jpg', 'vegetable' ),
  ( 'cherry', 'cherry.jpg', 'fruit' ),
  ( 'chicken', 'chicken.jpg', 'meat' ),
  ( 'corn', 'corn.jpg', 'vegetable' ),
  ( 'cucumber', 'cucumber.jpg', 'vegetable' ),
  ( 'eggplant', 'eggplant.jpg', 'vegetable' ),
  ( 'fish', 'fish.jpg', 'meat' ),
  ( 'flour', 'flour.jpg', 'other' ),
  ( 'ginger', 'ginger.jpg', 'other' ),
  ( 'green_bean', 'green_bean.jpg', 'vegetable' ),
  ( 'onion', 'onion.jpg', 'vegetable' ),
  ( 'orange', 'orange.jpg', 'fruit' ),
  ( 'pineapple', 'pineapple.jpg', 'fruit' ),
  ( 'potato', 'potato.jpg', 'vegetable' ),
  ( 'pumpkin', 'pumpkin.jpg', 'vegetable' ),
  ( 'raspberry', 'raspberry.jpg', 'fruit' ),
  ( 'red_pepper', 'red_pepper.jpg', 'vegetable' ),
  ( 'salt', 'salt.jpg', 'other' ),
  ( 'spinach', 'spinach.jpg', 'vegetable' ),
  ( 'strawberry', 'strawberry.jpg', 'fruit' ),
  ( 'sugar', 'sugar.jpg', 'other' ),
  ( 'tomato', 'tomato.jpg', 'vegetable' ),
  ( 'watermelon', 'watermelon.jpg', 'fruit' )
ON CONFLICT DO NOTHING;
```

This is the way to do many inserts at once, just by comma separating sets in the VALUES part. Note the ON CONFLICT section. Some of these you may have already inserted. This is telling PostgreSQL that if a row exists already to just do nothing about it. We could also do something like:

```sql
INSERT INTO ingredients (
  title, image, type
) VALUES
  ( 'watermelon', 'banana.jpg', 'this won''t be updated' )
ON CONFLICT (title) DO UPDATE SET image = excluded.image;
```

This is what many of us would call an "upsert". Insert if that title doesn't exist, update if it does. If you try to run that query (or the previous one) without the ON CONFLICT statement, it will fail since we asserted that title is a UNIQUE field. The type won't be updated because we didn't choose to handle that. Also notice we did two ' in a row.

```sql
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon';
```

```sql
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING id, title, image;
```

The RETURNING clause tells Postgres you want to return those columns of the things you've updated. In our case I had it return literally everything we have in the table so you could write that as:

```sql
UPDATE ingredients SET image = 'watermelon.jpg' WHERE title = 'watermelon' RETURNING *;
```

```sql
INSERT INTO ingredients
  (title, image)
VALUES
  ('not real 1', 'delete.jpg'),
  ('not real 2', 'delete.jpg');
```

```sql
UPDATE ingredients
SET image = 'different.jpg'
WHERE image='delete.jpg'
RETURNING id, title, image;
```

```sql
DELETE FROM ingredients
WHERE image='different.jpg'
RETURNING *;
```

```sql
SELECT id, title, image
FROM ingredients
LIMIT 5;
```

```sql
SELECT *
FROM ingredients
WHERE type = 'vegetable'
  AND id < 20;
```

```sql
SELECT *
FROM ingredients
WHERE id <= 10
  OR id >= 20;
```

```sql
SELECT * FROM ingredients ORDER BY id DESC;
```

```sql
SELECT * FROM ingredients WHERE title LIKE '%pota%'; -- Fuzzy matching
```

```sql
SELECT * FROM ingredients WHERE CONCAT(title, type) LIKE '%fruit%'; -- Combine two strings into one
```

```sql
SELECT * FROM ingredients WHERE LOWER(CONCAT(title, type)) LIKE LOWER('%FrUiT%'); -- LowerCase
```

```sql
SELECT * FROM ingredients WHERE CONCAT(title, type) ILIKE '%FrUiT%'; -- An easier way to perform case insensitive lookup
```

We've been surrounding the LIKE values with %. This is basically saying "match 0 to infinite characters". So with "%berry" you would match "strawberry" and "blueberry" but not "berry ice cream". Because the "%" was only at the beginning it wouldn't match anything after, you'd need "%berry%" to match both "strawberry" and "blueberry ice cream".

```sql
SELECT * FROM ingredients WHERE title ILIKE 'c%'; --  All titles that start with "c".
```

You can put % anywhere. "b%t" will match "bt", "bot", "but", "belt", and "belligerent". There also exists \_ which will match 1 and only one character. "b_t" will match "bot" and "but" but not "bt", "belt", or "belligerent".
