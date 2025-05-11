# Netflix Content Trend Analysis (SQL-Based EDA)
This project is part of the “Data Analysis Project,” focusing on SQL-based EDA for Netflix contents data. Over two weeks, leveraged real Netflix data to practice SQL and Excel for data preprocessing, analysis, and visualization, aiming to uncover meaningful insights into Netflix’s content strategies.
## Project Overview
### Motivation and Objectives
Netflix has rapidly grown into a leading global streaming platform. With the OTT (Over-the-Top) market expanding and competition intensifying, we chose Netflix’s dataset for hands-on SQL/Excel practice—rich enough for meaningful analysis and relevant for generating insights about OTT strategies.

Objectives:
In 2020, the production company found itself in urgent need of Netflix investment as competition in the OTT market continued to intensify. Securing support from a global platform had become essential for the company’s next big leap. Against this backdrop, the question emerged:


### “What kind of content proposal would capture Netflix’s attention and lead to funding in 2020?”

This project set out to explore that question through a data-driven approach, focusing on the following goals:

1. SQL-Based EDA Practice

2. Leverage real Netflix data to practice SQL queries for data cleaning, preprocessing, and aggregation.
Use Excel for generating visualizations (charts, graphs) to present findings clearly.
Insight Generation for Investment Pitches

3. Pinpoint content trends (e.g., which genres and audience ratings are gaining traction).
Uncover country-specific preferences, whether Netflix invests more in local TV shows or movies.
Determine key factors (e.g., cast size, director experience) that may influence content performance.
Strategic Recommendations

4. Recommend content directions (format, genre, target rating) that align with Netflix’s evolving strategies.
Formulate a data-supported pitch for why certain content might be more likely to secure Netflix’s investment.
To accomplish this, the team conducted SQL-based EDA on a large Netflix dataset—covering content type (Movies vs. TV Shows), country, date added, release year, genre listings, and age ratings. The storyline centers on the fictional scenario of “High Five-2” aiming to pitch the next big show or film to Netflix executives. By diving into factors that have historically guided Netflix’s content decisions, the project sheds light on which genres, formats, and age ratings might lead to higher viewer engagement and thus attract Netflix’s funding.

In sum, this project marries real data with a narrative of high-stakes content pitching, illustrating how strategic, data-driven analysis can inform crucial decisions in a highly competitive OTT landscape.


## Data Source and Description
Main Dataset: [“Netflix Shows and Movies - Exploratory Analysis”](https://www.kaggle.com/code/shivamb/netflix-shows-and-movies-exploratory-analysis/notebook) from Kaggle 

 + Columns include title, director, cast, country, date_added, release_year, rating, duration, listed_in (genre), etc.
 + Supplementary Dataset (for select analyses): IMDb scores and votes, used to examine potential correlations with cast size or director expertise.

 | Colomn Name  | Description |
| ------------- | ------------- |
| `Show_id`  | Unique ID for every Movie/TV show  |
| `Type`  | Identifier - A Movie or TV show |
| `Title`  | Title of the Movie/TV show  |
| `Director`  | Director of the Movie  |
| `Cast`  | Actors in the Movie/TV show  |
| `Country`  | Country where the movie/show was produced  |
| `Date_added`  | Date it was added on Netflix  |
| `Release_year`  | Actual Release year of the Movie/Show  |
| `Rating`  | TV rating of the Movie/Show  |
| `Duration`  | Total Duration in minutes  |
| `Listed_in`  | Genre  |
| `Description`  | The summary description  |



## Tools and Frameworks
 + SQL: Data cleansing, preprocessing, and feature extraction (e.g., missing-value handling, type conversions
 + Tableau: Data visualization, dashboard creation (charts, graphs)


## Data Preprocessing Workflow
-- Drop the table if it already exists to avoid conflicts
DROP TABLE IF EXISTS cleaned_netflix;

-- Create a cleaned version of the Netflix dataset
CREATE TABLE cleaned_netflix AS
SELECT
    show_id,
    title,
    -- Keep missing directors and cast entries, replacing NULLs with 'Unknown'
    COALESCE(director, 'Unknown') AS director,
    COALESCE(cast, 'Unknown') AS cast,
    COALESCE(country, 'Unknown') AS country,

    -- Convert date_added to ISO format (YYYY-MM-DD)
    CASE
        WHEN date_added IS NOT NULL THEN STRFTIME('%Y-%m-%d', date_added)
        ELSE NULL
    END AS date_added,

    release_year,

    -- Retain rating for classification analysis
    rating,

    duration,

    -- Extract numeric portion of duration (in minutes or seasons)
    CASE
        WHEN duration LIKE '% min' THEN CAST(SUBSTR(duration, 1, INSTR(duration, ' ') - 1) AS INT)
        WHEN duration LIKE '% Season%' THEN CAST(SUBSTR(duration, 1, INSTR(duration, ' ') - 1) AS INT)
        ELSE NULL
    END AS duration_num,

    listed_in,
    description,

    -- Replace NULL type values with the dataset's mode (most frequent type)
    (
        SELECT mode_type
        FROM (
            SELECT type AS mode_type,
                   COUNT(*) AS cnt
            FROM raw_netflix
            GROUP BY type
            ORDER BY cnt DESC
            LIMIT 1
        )
    ) AS type

FROM raw_netflix
-- Remove rows where critical fields are missing
WHERE rating IS NOT NULL
  AND date_added IS NOT NULL;



## Dashboard
[Dashboard link](https://public.tableau.com/app/profile/jiyoon.shin1127/viz/NetflixDashboards-blackver_/1)
![image](https://github.com/user-attachments/assets/a934abc4-bc89-4da9-8c8f-3d72dedf6910)


[Dashboard link](https://public.tableau.com/app/profile/jiyoon.shin1127/viz/NetflixDashboards_17466287074980/1)
![image](https://github.com/user-attachments/assets/415aa925-b2a2-40b8-9f7f-8b5c020686e7)
