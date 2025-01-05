# Ease-of-Doing-Business-India-vs-G7
This repository contains an in-depth analysis of global economic growth and development trends, with a focus on the ease of doing business and comparative insights between India and G7 countries.

This repository contains a comparative analysis of the Ease of Doing Business between India and G7 countries. The analysis focuses on key metrics such as “Starting a Business,” “Paying Taxes,” and overall Ease of Doing Business scores. Using data visualization and geographic mapping, this project highlights India’s challenges in bureaucracy, compliance costs, and procedural delays compared to the streamlined processes in G7 countries.

---

## Project Overview

### Objectives:
1. Identify critical gaps between India and G7 countries in Ease of Doing Business metrics.
2. Highlight challenges and suggest improvements in regulatory frameworks, tax systems, and technological adoption.
3. Provide actionable insights through data visualization and statistical analysis.

### Tools and Libraries Used:
- **R Libraries**:
  - `readxl` for data import.
  - `dplyr` for data manipulation.
  - `ggplot2` for data visualization.
  - `leaflet` for interactive mapping.
  - `sf`, `rnaturalearth`, `rnaturalearthdata` for geographic data.

---

## Data Analysis Steps

### Data Cleaning and Preparation
1. The raw dataset is imported using `readxl` and filtered to include only G7 countries and India.
2. Relevant columns such as Ease of Doing Business scores and related metrics (“Starting a Business,” “Paying Taxes”) are selected.
3. Missing values are removed, and a new column "Average Score" is calculated as the mean of all selected metrics for each country.

```r
cleaned_data <- raw_data %>%
  filter(Economy %in% c(g7_countries, "India")) %>%
  select(Economy, Region, `Ease of doing business score`,
         `Score-Starting a business`, `Score-Registering property`,
         `Score-Paying taxes`, `Score-Getting electricity`) %>%
  na.omit() %>%
  mutate(Average_Score = rowMeans(across(starts_with("Score")), na.rm = TRUE))
```

---

## Key Visualizations

### 1. Bar Plot: Ease of Doing Business Scores
- **Purpose:** Compare Ease of Doing Business scores across G7 countries and India.
- **Code:**

```r
ggplot(cleaned_data, aes(x = reorder(Economy, -`Ease of doing business score`), y = `Ease of doing business score`)) +
  geom_bar(stat = "identity", fill = "steelblue") +
  coord_flip() +
  labs(title = "Ease of Doing Business: G7 Countries vs. India",
       x = "Country",
       y = "Ease of Doing Business Score") +
  theme_minimal()
```

### 2. Box Plot: Starting a Business Scores
- **Purpose:** Show distribution of “Starting a Business” scores.
- **Code:**

```r
ggplot(cleaned_data, aes(x = Economy, y = `Score-Starting a business`)) +
  geom_boxplot(fill = "lightblue") +
  labs(title = "Starting a Business Scores: G7 Countries vs. India",
       x = "Country",
       y = "Score") +
  theme_minimal()
```

### 3. Line Plot: Trends Over Time
- **Purpose:** Analyze changes in Ease of Doing Business scores over the years.
- **Code:**

```r
ggplot(g7_india_trend, aes(x = `DB year`, y = Score, color = Economy, group = Economy)) +
  geom_line(linewidth = 1.2) +  
  geom_point(size = 3) +  
  facet_wrap(~ Metric, scales = "free_y") +  
  labs(title = "Ease of Doing Business Score Trends: G7 vs. India",
       x = "Year",
       y = "Score") +
  theme_minimal(base_size = 15)
```

### 4. Leaflet Map: Ease of Doing Business
- **Purpose:** Visualize scores geographically.
- **Code:**

```r
leaflet(merged_data) %>%
  addProviderTiles(providers$CartoDB.DarkMatter) %>%
  addPolygons(
    fillColor = ~pal(`Ease of doing business score`),  
    weight = 1,
    color = "white",  
    fillOpacity = 0.8
  ) %>%
  addLegend(
    position = "bottomright",
    pal = pal,
    values = ~`Ease of doing business score`,
    title = "Ease of Doing Business Score",
    opacity = 1
  )
```

---

## Key Findings

### Strengths of G7 Countries:
1. **Ease of Starting a Business:**
   - Streamlined regulatory processes.
   - Reduced steps, shorter timeframes, and lower compliance costs.

2. **Paying Taxes:**
   - Efficient tax systems with minimal documentation and lower overall costs.

### Challenges for India:
1. **High Bureaucracy and Compliance Costs:**
   - Lengthy processes and excessive documentation.

2. **Complex and Time-Consuming Procedures:**
   - Significant delays in business registration and tax compliance.

### Recommendations for India:
1. **Simplify Regulatory Frameworks:**
   - Reduce procedural hurdles and costs.
2. **Adopt Technology:**
   - Implement digital solutions for business registration and tax compliance.
3. **Emulate G7 Best Practices:**
   - Streamline processes and foster a conducive business environment.

---

## Conclusion
This analysis identifies key areas where India lags behind G7 countries and provides actionable recommendations to improve its Ease of Doing Business ranking. Adopting streamlined regulatory practices, leveraging technology, and reducing compliance costs are critical for fostering a competitive business environment in India.

---

## Repository Structure
- **Data**: Contains raw and cleaned datasets.
- **Scripts**: Includes R scripts for data processing and visualization.
- **Visualizations**: Output charts and maps.
- **Reports**: Detailed findings and recommendations.

