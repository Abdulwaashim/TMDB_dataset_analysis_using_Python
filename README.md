# TMDB Movie Data Analysis

## Project Overview
This project analyzes a dataset from The Movie Database (TMDB), containing around 1 million rows of movie-related data. The analysis explores trends in movie popularity, revenue, and genre performance over time.

## Dataset
- **Source:** TMDB dataset
- **Size:** ~1 million rows
- **Key Features:** Movie title, release date, budget, revenue, popularity, genre, and production company.

## Analysis Steps
1. **Data Cleaning**
   - Handling missing values
   - Removing duplicates
   - Data type conversions
   - Filtering relevant data for analysis
2. **Exploratory Data Analysis (EDA)**
   - Understanding distributions and trends
   - Identifying outliers and anomalies
3. **Data Visualization & Insights**
   - Budget vs. Revenue analysis
   - Top 10 most popular movie genres
   - Trends in movie release dates & revenue
   - Relationship between budget, revenue & popularity
   - Popularity of movie genres over time
   - Identifying successful production companies
   - Top 10 revenue-generating movies

## Key Insights
- **High-budget movies tend to generate higher revenue**, but not always; factors like marketing and audience reception also play a role.
- **Action, Adventure, and Sci-Fi movies dominate revenue generation**, while Drama and Comedy are the most frequently produced genres.
- **Revenue trends indicate that blockbuster movies tend to perform exceptionally well in specific months**, particularly during summer and holiday seasons, aligning with peak audience engagement periods.
- **Production companies like Disney, Warner Bros., and Universal Pictures consistently produce high-revenue films.**
- **Movies with a higher popularity score tend to perform better financially, indicating a correlation between pre-release buzz and box office success.**

## Recommendations
- **For Investors & Producers:** Focus on high-budget Action, Sci-Fi, and Adventure genres to maximize revenue potential.
- **For Production Planning:** Schedule movie releases during peak seasons (summer & holidays) to optimize box office performance.
- **For Marketing Teams:** Invest in boosting pre-release popularity through promotional campaigns and audience engagement strategies.
- **For Data Analysts:** Further exploration can include sentiment analysis on reviews to understand audience preferences.

## Tools & Libraries Used
- **Python**: Pandas, Matplotlib, Seaborn
- **Jupyter Notebook**

## Code Implementation
### Importing Libraries & Loading Dataset
```python
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

file_path = "TMDB_movie_dataset.csv"
df = pd.read_csv(file_path)
df.head()
```
### Data Cleaning
```python
# Checking for missing values
missing_values = df.isnull().sum()
print("Missing Values:\n", missing_values)

# Checking for duplicate rows
duplicates = df.duplicated().sum()
print("\nNumber of Duplicate Rows:", duplicates)

# Removing duplicate rows
df = df.drop_duplicates()

# Dropping irrelevant columns
df = df.drop(columns=["backdrop_path", "homepage", "tagline", "keywords"])

# Handling missing values
df = df.dropna(subset=["title", "release_date", "original_language", "genres"])
df["runtime"] = df["runtime"].fillna(df["runtime"].median())
df["overview"] = df["overview"].fillna("No description available")

# Filling missing categorical values
df[["genres", "production_companies", "production_countries", "spoken_languages"]] = \
    df[["genres", "production_companies", "production_countries", "spoken_languages"]].fillna("Unknown")

# Final check for missing values
missing_values = df.isnull().sum()
print("Missing Values after cleaning:\n", missing_values)
```
### Budget vs. Revenue Analysis
```python
plt.figure(figsize=(10, 6))
sns.scatterplot(x=df['budget'], y=df['revenue'])
plt.xlabel("Budget")
plt.ylabel("Revenue")
plt.title("Budget vs Revenue")
plt.show()
```
### Top 10 Most Popular Genres
```python
genre_counts = df['genres'].value_counts().head(10)
genre_counts.plot(kind='bar', figsize=(10,5), color='skyblue')
plt.xlabel("Genre")
plt.ylabel("Count")
plt.title("Top 10 Most Popular Movie Genres")
plt.show()
```
### Identifying Successful Production Companies
```python
production_companies = df.groupby("production_company")["revenue"].sum().sort_values(ascending=False).head(10)
production_companies.plot(kind='bar', figsize=(10,5), color='lightcoral')
plt.xlabel("Production Company")
plt.ylabel("Total Revenue")
plt.title("Top Revenue-Generating Production Companies")
plt.show()
```
### Trends in Movie Release Dates & Revenue
```python
df['release_year'] = pd.to_datetime(df['release_date']).dt.year
release_trends = df.groupby('release_year')['revenue'].sum()

plt.figure(figsize=(12, 6))
plt.plot(release_trends.index, release_trends.values, marker='o', linestyle='-', color='b')
plt.xlabel("Year")
plt.ylabel("Total Revenue")
plt.title("Trends in Movie Release Dates & Revenue")
plt.grid()
plt.show()
```
### Relationship Between Budget, Revenue & Popularity
```python
plt.figure(figsize=(10, 6))
sns.scatterplot(x=df['budget'], y=df['revenue'], hue=df['popularity'], size=df['popularity'], sizes=(20, 200), palette='viridis')
plt.xlabel("Budget")
plt.ylabel("Revenue")
plt.title("Relationship Between Budget, Revenue & Popularity")
plt.legend(title="Popularity", bbox_to_anchor=(1, 1))
plt.show()
```
### Popularity of Movie Genres Over Time
```python
df['decade'] = (df['release_year'] // 10) * 10
genre_trends = df.groupby(['decade', 'genres']).size().reset_index(name='count')

plt.figure(figsize=(12, 6))
sns.lineplot(data=genre_trends, x='decade', y='count', hue='genres', marker="o")
plt.title("Popularity of Movie Genres Over Time")
plt.xlabel("Decade")
plt.ylabel("Number of Movies")
plt.legend(title="Genre", bbox_to_anchor=(1,1))
plt.show()
```
### Top 10 Revenue-Generated Movies
```python
top_revenue_movies = df[['title', 'revenue']].sort_values(by='revenue', ascending=False).head(10)

plt.figure(figsize=(10, 5))
plt.barh(top_revenue_movies['title'], top_revenue_movies['revenue'], color='skyblue')
plt.xlabel("Revenue (in billions)")
plt.ylabel("Movie Title")
plt.title("Top 10 Revenue-Generated Movies")
plt.gca().invert_yaxis()
plt.show()
```

## How to Use
1. Clone this repository:
   ```sh
   git clone [https://github.com/yourusername/tmdb-movie-analysis.git](https://github.com/Abdulwaashim/TMDB_dataset_analysis_using_Python)
   ```
2. Install dependencies:
   ```sh
   pip install pandas matplotlib seaborn
   ```
3. Open and run the Jupyter Notebook:
   ```sh
   jupyter notebook tmdb_data_analysis.ipynb
   ```

## Future Improvements
- Integrate machine learning models to predict movie success.
- Expand analysis with external datasets for deeper insights.
- Perform sentiment analysis on movie reviews for better audience understanding.

## Author
[Abdul waashim] - Data Analyst
