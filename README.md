# Himalia
Repository for the Water Wise app concept, developed by the Himalia Team in the Nasa Space Apps Challenge 2024.

## Overview

This application was developed to encourage water conservation through gamification, using data provided by NASA, such as drought indices, surface temperatures, and meteorological data collected via web scraping. The app sends personalized alerts to users based on their water consumption compared to historical patterns and current weather conditions in their region.

---

## Project Structure

```bash
.
├── README.md           # Project overview and setup instructions
├── src/                # Source code
│   ├── api/            # API implementations
│   ├── scraping/       # Web scraping modules for data extraction
│   ├── models/         # Database models
│   ├── controllers/    # Business logic controllers
├── docs/               # Technical documentation
│   ├── architecture.md # System architecture and components
│   ├── api_docs.md     # API documentation
│   ├── db_schema.md    # Complete database schema
│   ├── conceptual_db.md# Conceptual database design
│   ├── scraping_docs.md# Web scraping process documentation
│   ├── technologies.md # Technologies and tools used
│   ├── references.md   # External references and links
├── LICENSE             # License for the project
```

---

## Technical Documentation

### 1. System Architecture

The system follows a distributed microservices architecture, with data acquisition being done through **web scraping** from NASA websites. Business logic and data processing are handled in the backend using **Flask**, while the user interface is developed using modern frameworks for a responsive and accessible experience.

---

### 2. Conceptual Database Design

#### Users Table
This table stores user information and their respective water consumption patterns.

```sql
CREATE TABLE users (
    user_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    email VARCHAR(255) NOT NULL UNIQUE,
    password_hash VARCHAR(255) NOT NULL,
    address VARCHAR(255),
    average_water_consumption DECIMAL(10,2),
    current_water_consumption DECIMAL(10,2),
    points INT DEFAULT 0,
    alerts_enabled BOOLEAN DEFAULT TRUE
);
```

#### Scraped Data Table
This table stores the environmental data scraped from NASA.

```sql
CREATE TABLE scraped_data (
    data_id SERIAL PRIMARY KEY,
    region VARCHAR(255),
    drought_index DECIMAL(5,2),
    temperature DECIMAL(5,2),
    precipitation DECIMAL(5,2),
    date DATE NOT NULL
);
```

#### Water Consumption Table
This table tracks user water consumption on a daily basis.

```sql
CREATE TABLE water_consumption (
    consumption_id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(user_id),
    date DATE NOT NULL,
    water_consumed DECIMAL(10,2),
    comparison_to_average DECIMAL(10,2)
);
```

#### Achievements Table
This table records user achievements for gamification purposes.

```sql
CREATE TABLE achievements (
    achievement_id SERIAL PRIMARY KEY,
    user_id INT REFERENCES users(user_id),
    achievement_description VARCHAR(255),
    points_awarded INT,
    date DATE
);
```

---

### 3. Data Extraction via Web Scraping

The data will be acquired via **web scraping** from NASA websites that provide public information but lack direct APIs. We will use tools like **BeautifulSoup** and **Selenium** to automate navigation and extract relevant data.

#### Web Scraping Steps:
1. **Identifying Source Pages**: NASA websites that offer relevant data, such as those provided by **NASA POWER**, **Giovanni**, and **SEDAC**.
2. **Automating Scraping**: Using **Selenium** for navigating dynamic pages and **BeautifulSoup** for extracting relevant data, such as temperature, drought indices, and precipitation.
3. **Data Cleaning and Storage**: After extraction, the data is cleaned and stored in the database for further analysis.
4. **Frequency**: Scraping will be executed on a scheduled basis (e.g., every 24 hours) to keep the data updated.

---

### 4. External References and Data Sources

Here are the primary external data sources used in the project, with links for reference:

1. **NASA POWER API**: 
   - Provides meteorological data, including temperature, humidity, and solar radiation.
   - [NASA POWER API Documentation](https://power.larc.nasa.gov/docs/services/api/application/)
   
2. **MODIS Land Surface Temperature (NASA)**:
   - Provides land surface temperature data for use in drought monitoring and comparison to user water consumption.
   - [MODIS Data](https://neo.gsfc.nasa.gov/view.php?datasetId=MOD_LSTAD_M)

3. **SEDAC Drought Hazard Frequency Distribution (Columbia University)**:
   - Provides historical drought frequency data, useful for identifying drought-prone areas and triggering alerts.
   - [SEDAC Data](https://sedac.ciesin.columbia.edu/data/set/ndh-drought-hazard-frequency-distribution)

4. **Giovanni Platform**:
   - An interactive tool to visualize and analyze NASA’s environmental data, including drought indices and precipitation.
   - [Giovanni Tool](https://giovanni.gsfc.nasa.gov/giovanni)

---

### 5. Back-End Framework: Flask

The back-end will be developed in **Python** using the **Flask** framework.

#### Why Flask?
- **Flask** is a lightweight, flexible microframework ideal for building APIs and small-scale applications.
- **Pros**:
  - Minimal setup and overhead, making it ideal for microservices and modular applications.
  - Easily integrates with libraries like **SQLAlchemy** for database management.
  - Simplicity and flexibility, allowing the team to focus on specific features without the need for extensive configuration.

**Flask** was chosen for this project due to its lightweight nature and the simplicity it offers for building RESTful APIs, which align with the goals of the project.

---

### 6. Front-End Framework

The front-end will be developed using **React Native** for cross-platform mobile development (Android and iOS).

#### Why React Native?
- **Cross-Platform**: A single codebase for both Android and iOS platforms.
- **Large Community**: React Native has a vibrant community, with many reusable libraries and components.
- **Native Performance**: It provides a near-native experience in terms of performance.

#### Additional Tools:
- **Expo**: A toolchain for React Native that simplifies the development process, especially useful for prototyping and quick iterations.
- **Axios**: For handling HTTP requests to communicate with the back-end.
- **React Navigation**: For managing navigation within the app.

---

### Summary of Key Decisions

1. **Database**: PostgreSQL was chosen for its robustness, geospatial data support, and scalability.
2. **Data Source**: Data will be collected from NASA using web scraping (NASA POWER, MODIS LST, SEDAC, Giovanni).
3. **Data Extraction**: Web scraping using Python tools (BeautifulSoup, Selenium).
4. **Back-End**: **Python** with **Flask** as the chosen framework.
5. **Front-End**: React Native for cross-platform mobile app development.
