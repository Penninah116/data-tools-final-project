
# Data-Analysis

<div align="center">
  <img width="200" height="200" alt="Event Ticketing Logo" src="https://github.com/user-attachments/assets/20661293-a214-4004-9042-657102fb0710" />
  <br/>
  <h2><b>Event Ticketing Project </b></h2>
</div>

# 📗 Table of Contents

* [📖 About the Project](#about-project)
  * [🛠 Built With](#built-with)
  * [Key Features](#key-features)
  * [🚀 Live Demo](#live-demo)
* [💻 Getting Started](#getting-started)
  * [Prerequisites](#prerequisites)
  * [Setup](#setup)
  * [Usage](#usage)
  * [Connecting from Posit to Supabase](#posit-supabase-connection)
* [💾 Schema SQL](#schema-sql)
* [📊 R Data Analysis](#r-data-analysis)
* [📖 Data Dictionary](#data-dictionary)
* [👥 Authors](#authors)
* [🔭 Future Features](#future-features)
* [🤝 Contributing](#contributing)
* [⭐️ Show your support](#support)
* [🙏 Acknowledgements](#acknowledgements)
* [❓ FAQ](#faq)
* [📝 License](#license)

---

# 📖 About the Project <a name="about-project"></a>

> This project models a music streaming platform's backend database. It includes users, artists, songs, and user favorites, enabling functionalities like song liking, artist categorization, and user engagement tracking. Additionally, we demonstrate R-based data analysis of user activity and artist performance.

## 🛠 Built With <a name="built-with"></a>

### Tech Stack

<details>
  <summary>Database & Hosting</summary>
  <ul>
    <li><a href="https://supabase.com">Supabase (PostgreSQL)</a> – backend database for tables, data storage, and queries</li>
  </ul>
</details>

<details>
  <summary>SQL Queries</summary>
  <ul>
    <li>Database schema creation, data insertion, and example queries</li>
  </ul>
</details>

<details>
  <summary>R Data Analysis</summary>
  <ul>
    <li><a href="https://posit.co/">Posit / RStudio</a> for connecting to Supabase and performing exploratory data analysis (EDA)</li>
    <li>Libraries: DBI, dplyr, ggplot2 for querying and visualization</li>
  </ul>
</details>

### Key Features <a name="key-features"></a>

* Users can like multiple songs and track favorites.
* Songs are linked to artists, supporting multiple songs per artist.
* SQL queries for analyzing top songs, active users, and most popular artists.
* R-based visualizations for song popularity, user activity, and artist performance.

<p align="right"><a href="#about-project">back to top</a></p>

## 🚀 Live Demo <a name="live-demo"></a>

> Backend-only project. Interact via Supabase SQL editor.

* [Supabase Project Link](https://supabase.com/dashboard/project/octmhkzbzxsoaegmuaei/sql/3cf2fb04-a61c-4254-87aa-e725d2b6f0f9)

<p align="right"><a href="#about-project">back to top</a></p>

---

# 💻 Getting Started <a name="getting-started"></a>

### Prerequisites

* Supabase account
* Posit / RStudio
* R packages: DBI, dplyr, ggplot2

### Setup

Clone the repository:

```bash
git clone https://github.com/Evans-dotcom/event-ticketing-data-analysis.git
cd event-ticketing-data-analysis
```

### Usage

1. Open Supabase and create a new project.
2. Access the SQL editor and execute `schema.sql` to create tables and insert sample data:

```sql
\i schema.sql
```

3. A quick taste of how R posit code would look like:

```r
# Load connection
library(DBI)
connect_db <- function() {
  dbConnect(
    RPostgres::Postgres(),
    dbname = "postgres",
    host = "aws-1-eu-north-1.pooler.supabase.com",
    port = 5432,
    user = "postgres.pwsbzyjjqwxtqzzpaghy",
    password = "usA-wt4/$Gg4x#m",
    sslmode = "require"
  )
}

source("Data-Analysis.R")
con <- connect_db()
dbListTables(con)

# Run your query
# 1. Event popularity (number of tickets sold)
event_sales <- dbGetQuery(con, "
  SELECT e.event_name, COUNT(t.ticket_id) AS tickets_sold
  FROM events e
  LEFT JOIN tickets t ON e.event_id = t.event_id
  GROUP BY e.event_name
  ORDER BY tickets_sold DESC;
")
# 2. Top customers (users with most tickets)
top_users <- dbGetQuery(con, "
  SELECT u.full_name, COUNT(t.ticket_id) AS tickets_bought
  FROM users u
  JOIN tickets t ON u.user_id = t.user_id
  GROUP BY u.full_name
  ORDER BY tickets_bought DESC;
")
#outome : 
# source("/cloud/project/alice_favorite.R")
#username              title artist_name
#1    alice Programmers choice   Sauti Sol

```
# Outcome upon running the code

<img width="1895" height="829" alt="image" src="https://github.com/user-attachments/assets/9f6ff072-74d4-4295-b415-14b43196043b" />
---

### Connecting from Posit to Supabase <a name="posit-supabase-connection"></a>

1. Install required R packages:

```r
install.packages(c("DBI", "RPostgres", "dplyr", "ggplot2"))
```

2. Create a `Data-Analysis.R` file:

```r
library(DBI)
connect_db <- function() {
  dbConnect(
    RPostgres::Postgres(),
    dbname = "postgres",
    host = "aws-1-eu-north-1.pooler.supabase.com",
    port = 5432,
    user = "postgres.pwsbzyjjqwxtqzzpaghy",
    password = "usA-wt4/$Gg4x#m",
    sslmode = "require"
  )
}
```

3. Use this connection in R scripts:

```r
source("Data-Analysis.R")
con <- connect_db()
dbListTables(con)
```

---
# Outcome after establishing connection
<img width="1891" height="868" alt="image" src="https://github.com/user-attachments/assets/22fa2585-d9ba-4aa8-bf4a-de0b87d93e81" />
---



# 💾 Must Have Schema SQL <a name="schema-sql"></a>


<details>
  <summary>Click to expand the full schema.sql that you must run in supabase before you create a conection to posit studi</summary>

```sql
-- Users table
CREATE TABLE Customers (
  user_id SERIAL PRIMARY KEY,
  full_name VARCHAR(100),
  email VARCHAR(100) UNIQUE NOT NULL,
  signup_date DATE DEFAULT CURRENT_DATE
);

-- Events table
CREATE TABLE events (
  event_id SERIAL PRIMARY KEY,
  event_name VARCHAR(100),
  event_date DATE,
  venue VARCHAR(100),
  organizer VARCHAR(100)
);

-- Tickets table
CREATE TABLE tickets (
  ticket_id SERIAL PRIMARY KEY,
  event_id INT REFERENCES events(event_id),
  user_id INT REFERENCES users(user_id),
  price DECIMAL(10,2),
  purchase_date DATE DEFAULT CURRENT_DATE
);

-- Insert users
INSERT INTO Customers (full_name, email) VALUES
('Alice Wanjiku', 'alice@gmail.com'),
('Brian Otieno', 'brian@gmail.com'),
('Carol Mwende', 'carol@gmail.com'),
('David Kamau', 'david@gmail.com'),
('Evelyne Njeri', 'evelyne@gmail.com');

-- Insert events
INSERT INTO events (event_name, event_date, venue, organizer) VALUES
('Tech Summit 2025', '2025-11-20', 'KICC', 'Micropoint Systems'),
('Music Fiesta', '2025-12-05', 'Uhuru Gardens', 'Sauti Nation'),
('Startup Pitch Night', '2025-12-10', 'Sarova Hotel', 'Pinnoserv'),
('AI Innovation Expo', '2025-12-15', 'Radisson Blu', 'TechHub Africa'),
('Cultural Gala', '2025-12-22', 'Bomas of Kenya', 'Heritage Org');

-- Insert tickets
INSERT INTO tickets (event_id, user_id, price) VALUES
(1, 1, 2000), (1, 2, 2000),
(2, 3, 1500), (2, 4, 1500),
(3, 1, 1800), (3, 5, 1800),
(4, 2, 2500), (4, 5, 2500),
(5, 3, 1000), (5, 4, 1000);

-- Example query: list tickets per user
SELECT u.full_name, e.event_name, t.price
FROM tickets t
JOIN users u ON t.user_id = u.user_id
JOIN events e ON t.event_id = e.event_id;
-- Example query
SELECT * FROM user_favorites;
```

```sql
-- Example query: list tickets per user
SELECT u.full_name, e.event_name, t.price
FROM tickets t
JOIN users u ON t.user_id = u.user_id
JOIN events e ON t.event_id = e.event_id;
```

</details>

<p align="right"><a href="#about-project">back to top</a></p>

---

# 📊 R Data Analysis <a name="r-data-analysis"></a>

<details>
<summary>Click to expand full R analysis code</summary>

```r
source("connect_db.R")
library(DBI)
library(dplyr)
library(ggplot2)

con <- connect_db()

# 1. Event popularity (number of tickets sold)
event_sales <- dbGetQuery(con, "
  SELECT e.event_name, COUNT(t.ticket_id) AS tickets_sold
  FROM events e
  LEFT JOIN tickets t ON e.event_id = t.event_id
  GROUP BY e.event_name
  ORDER BY tickets_sold DESC;
")
ggplot(event_sales, aes(x = reorder(event_name, tickets_sold), y = tickets_sold, fill = event_name)) +
  geom_col(show.legend = FALSE) + coord_flip() +
  labs(title = 'Most Popular Events', x = 'Event', y = 'Tickets Sold') +
  theme_minimal()

# 2. Revenue per event
revenue_event <- dbGetQuery(con, "
  SELECT e.event_name, SUM(t.quantity) AS total_revenue
  FROM tickets t
  JOIN events e ON t.event_id = e.event_id
  GROUP BY e.event_name;
")
ggplot(revenue_event, aes(x = reorder(event_name, total_revenue), y = total_revenue, fill = event_name)) +
  geom_col(show.legend = FALSE) + coord_flip() +
  labs(title = 'Revenue by Event', x = 'Event', y = 'Total Revenue (KSh)') +
  theme_minimal()

# 3. Daily Ticket Sales Trend
ticket_trend <- dbGetQuery(con, "
  SELECT DATE(t.purchase_date) AS purchase_date, SUM(t.quantity) AS tickets_sold
  FROM tickets t
  GROUP BY DATE(t.purchase_date)
  ORDER BY purchase_date;
")
ggplot(ticket_trend, aes(x = purchase_date, y = tickets_sold)) +
  geom_line(color = '#0073C2FF', size = 1.2) +
  geom_point(color = '#E69F00', size = 2) +
  labs(title = 'Daily Ticket Sales Trend', x = 'Date',y = 'Tickets Sold') +
  theme_minimal()
# 4.Customer Distribution by City
geo_dist <- dbGetQuery(con, "
  SELECT c.city, COUNT(DISTINCT c.customer_id) AS total_customers FROM customers c JOIN tickets t ON c.customer_id = t.customer_id  GROUP BY c.city
  ORDER BY total_customers DESC;
")
ggplot(geo_dist, aes(x = reorder(city, total_customers), y = total_customers, fill = city)) +
  geom_col(show.legend = FALSE) +
  coord_flip() +
  labs(
    title = 'Customer Distribution by City',
    x = 'City',
    y = 'Number of Customers'
  ) +
  theme_minimal()

```
</details>

### Most Popular Events
<img width="1888" height="857" alt="image" src="https://github.com/user-attachments/assets/25e48f5e-a363-4ed9-bff0-ea2394d7ba02" />

### Revenue Per Event

<img width="1895" height="879" alt="image" src="https://github.com/user-attachments/assets/584a1570-a7be-4c14-8909-5e0c093feb78" />


### Daily Ticket Sales Trend
<img width="1917" height="906" alt="image" src="https://github.com/user-attachments/assets/68d5e0a2-6148-497c-b7f8-7b9090209f65" />

### Customer Distribution By City
<img width="1920" height="889" alt="image" src="https://github.com/user-attachments/assets/c59a30e2-cdb9-4d20-9f69-0c32cad451a2" />


<p align="right"><a href="#about-project">back to top</a></p>

---

# 📖 Data Dictionary <a name="data-dictionary"></a>

**📖 Full Data Dictionary:** [Check it here](https://github.com/DENNIS-MURITHI/Data-Tools/blob/test_branch/data_dictionary.md)

<p align="right"><a href="#about-project">back to top</a></p>

---

# 👥 Authors <a name="authors"></a>

👤 **Evans Kibet**

* GitHub: [@EvansKibet](https://github.com/evans-dotcom)
* LinkedIn: [LinkedIn](https://www.linkedin.com/in/evans-langat-680b05342/)

<p align="right"><a href="#about-project">back to top</a></p>

---

# 🔭 Future Features <a name="future-features"></a>

* Front-end integration with Event Ticketing Project  
* Advanced analytics (top songs, popular artists, trends)  
* Playlists, ratings, and user-generated content
* 
<p align="right"><a href="#about-project">back to top</a></p>

---

# 🤝 Contributing <a name="contributing"></a>

Contributions, issues, and feature requests are welcome. Open an issue or submit a pull request.

<p align="right"><a href="#about-project">back to top</a></p>

---

# ⭐️ Show your support <a name="support"></a>

If you like this project, give it a ⭐️ on GitHub!

<p align="right"><a href="#about-project">back to top</a></p>

---

# 🙏 Acknowledgements <a name="acknowledgements"></a>

* [Supabase](https://supabase.com/) for PostgreSQL hosting and testing  
* [Posit](https://docs.posit.co/connect/) Connect Documentation   

<p align="right"><a href="#about-project">back to top</a></p>

---

# ❓ FAQ <a name="faq"></a>

**1. How do I run this project in Posit?**  
Open the repository in **Posit (RStudio)**, install dependencies, and run the R scripts step by step.  
Make sure your Supabase credentials are set correctly in `connect_db.R`.

**2. What dependencies are needed?**  
Install the following R packages:  
```r
install.packages(c("DBI", "RPostgres", "dplyr", "ggplot2"))
```
### 3. Can I use MySQL or other databases?  
❌ **No.** This project connects only to **Supabase (PostgreSQL)** for consistency and compatibility with R and Posit.

---

### 4. How do I connect Posit to Supabase?  
Use the `DBI` and `RPostgres` packages along with your Supabase credentials found in:  
**Supabase → Project Settings → Database → Connection Info**  

# 📝 License <a name="license"></a>

This project is licensed under MIT License - see [LICENSE](LICENSE) for details.
