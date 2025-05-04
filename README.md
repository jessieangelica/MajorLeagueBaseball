### Problem 1

Objective:
- Using the Lahman Batting dataset, We aim to compare player batting performance during two significant periods in baseball history:
  - The Steroid Era (1994–2005)
  - The Recent Era (2012–2023)
- We'll use key batting statistics—Hits (H), At-Bats (AB), Home Runs (HR)—and calculate Batting Average (BA) to visualize and analyze player performance across the eras.

#### Step 1: Load Required Libraries and Dataset
- We start by importing necessary Python libraries for data manipulation (pandas, numpy) and visualization (matplotlib, seaborn).
- We load the Batting.csv file from the Lahman database.

#### Step 2: Data Cleaning & Preparation
2.1 Check for Duplicate Records Based on Player, Year, and Team
- Remove duplicate rows by considering a combination of:
  - playerID: Unique player identifier
  - yearID: The season year
  - teamID: The team the player was associated with in that year
- It accounts for scenarios where a player may have played for multiple teams within the same season. By doing this, we prevent duplicate aggregations for players who switched teams mid-season.

2.2 Select Relevant Columns
- We ensure that essential columns for analysis exist in the dataset. These include:
  - yearID: Season year
  - AB: At-bats
  - H: Hits
  - HR: Home runs
  - playerID: Unique player identifier
- If any required columns are missing, We raise an error.

2.3 Filter Out Low-Participation Players
- I exclude players with fewer than 50 at-bats in a season to reduce statistical noise and focus on regular contributors.

2.4 Calculate Batting Average (BA)
- We compute Batting Average (BA), a key measure of player performance: BA = Hits / At-Bats

#### Step 3: Create Era-Based Subsets
We segment the dataset into two eras:
- Steroid Era: 1994–2005
- Recent Era: 2012–2023

These ranges reflect different policy environments around performance-enhancing drug use and modern baseball strategies.

#### Step 4: Label and Combine Era Data
We add a new column (Era) to label each record with its corresponding era, and then combine both subsets into one dataframe for analysis.

#### Step 5: Reshape Data for Time-Series Comparison
We reshape the data to wide format to analyze annual trends for each player. This structure allows for:
- Cross-year comparisons
- Temporal performance tracking
- Easier visualization and statistical modeling

Each statistic (H, HR, AB, BA) is now organized by year for every player, enabling longitudinal analysis.

#### Step 6: Visualizing Player Performance Metrics Across Eras
To compare player performance between the Steroid Era and the Recent Era, we visualize the distribution and central tendency of key batting statistics:
- Batting Average (BA)
- Hits (H)
- Home Runs (HR)
- At-Bats (AB)

We use Seaborn’s histplot() with:
- kde=True: Adds a Kernel Density Estimation curve for smoother insights.
- element='step': Uses unfilled line-style bars for easier comparison.
- hue='Era': Color-coded by era for side-by-side visual comparisons.
- stat='density': Ensures the area under each curve sums to 1, making the shapes directly comparable regardless of sample size.

6.1 Histograms with KDE – Distribution Comparison
- This view helps reveal changes in performance distributions, such as shifts in average values, spread, or skewness (e.g., whether they counts were more extreme in one era).
- Each subplot above represents the distribution of a batting metric, separated by era. Key takeaways:
- Batting Average (BA)
  - Both eras exhibit a roughly normal distribution, centered around .25.
  - The Steroid Era shows a slightly higher central tendency, with a heavier tail toward higher BA values.
  - The Recent Era appears more symmetrical and tighter around the median.
- Hits (H)
  - Distributions are right-skewed in both eras.
  - The Steroid Era has a slightly longer tail, indicating more players with high hit counts.
  - The Recent Era shows a steeper drop-off, suggesting fewer extreme performances.
- Home Runs (HR)
  - Both distributions are positively skewed, with long tails.
  - The Steroid Era has more frequent higher HR values, supporting anecdotal evidence about enhanced power hitting during that period.
- At-Bats (AB)
  - Similar right-skewed distribution in both eras.
  - The Steroid Era shows a slightly broader distribution, with more players having a higher number of ABs — possibly due to longer tenures or reduced rotation.

6.2 Boxplots – Summary of Central Tendency and Spread
- We visualize the spread and outliers using boxplots for each statistic:
  - Boxes represent interquartile range (IQR)
  - Horizontal line shows the median
  - Whiskers and points highlight outliers and distribution tails
- Boxplots are especially useful for identifying differences in medians, presence of extreme values, and overall variance across eras. 
- The boxplots visualize median, IQR, and outliers for each batting metric by era.
- BA:
  - Median BA is marginally higher in the Steroid Era.
  - Both eras show similar IQR and range, with many outliers in both directions.
- Hits & AB:
  - Medians are slightly higher in the Steroid Era.
  - Wider spread in the Steroid Era, suggesting more variability.
- Home Runs (HR):
  - Similar median, but the Steroid Era has more extreme outliers — aligns with historical context around power hitting.

We generate summary statistics for BA, H, and HR across both eras:
- mean: Average performance
- min / max: Performance bounds
- std: Standard deviation (spread/variability)
- median: Central value

This quantitative snapshot gives us a numerical basis to support or challenge visual findings.

Why exclude AB?
- It is often more useful to normalize performance (e.g., BA = H/AB) than to compare AB directly, as it reflects opportunity more than efficiency.
- BA is marginally higher in the Steroid Era, but variability (std) is similar.
- Hits were slightly more common and extreme in the Steroid Era.
- Home Runs have similar means, but more extreme outliers in the Steroid Era, suggesting standout sluggers.

#### Conclusion :
- This exploratory analysis supports the hypothesis that:
- The Steroid Era may have featured more power hitters, higher volume of hits, and a marginally better BA.
- However, overall central tendencies are not drastically different, suggesting modern players may be performing similarly — possibly due to advances in training, analytics, and strategy.
- The shape of the distributions, particularly for HR and H, reveals subtle but telling shifts in player performance profiles.

### Problem 2
- Objective: Analyze the performance of Major League Baseball (MLB) players who were named in the Mitchell Report, which investigated the use of performance-enhancing drugs (PEDs), specifically steroids, in professional baseball.
- We'll compare the batting performance of players mentioned in the Mitchell Report to those not mentioned, focusing on Batting Average (BA) and Home Runs (HR) for the seasons between 1994 and 2005.

#### Step 1: Scraping the Mitchell Report Player List from Wikipedia
We first fetch and parse the HTML content of the Mitchell Report Wikipedia page to extract the list of players.

#### Step 2: Cleaning Player Names from the Mitchell Report
To match names from the Mitchell Report with the Lahman dataset, we need to clean and normalize them. This includes removing accents, punctuation, and inconsistent spacing.
- Check for missing values and data types

#### Step 3: Loading the Lahman People Dataset
We load the People.csv file from the Lahman dataset to get player IDs and match them with names from the Mitchell Report.

#### Step 4: Fuzzy Matching for Name Alignment
Due to name formatting inconsistencies, we use fuzzy matching (token sort ratio) to identify close matches between Mitchell Report names and those in the People dataset.

#### Step 5: Manual Fixes for Known Mismatches
A few names (typically 3–4) require manual correction. We apply known mappings based on visual inspection and domain knowledge.

#### Step 6: Merging Mitchell Report Names with Player IDs
We merge the cleaned Mitchell Report names with the Lahman People dataset to extract the corresponding playerID, which is used to join with the Batting dataset.

#### Step 7: Load and Preprocess the Batting Dataset

#### Step 8: Clean and Filter the Batting Data
To ensure meaningful analysis:
- We drop duplicate rows (some players may have multiple entries for the same season and team).
- We filter players with at least 50 At Bats (AB) to ensure statistical significance.
- We filter the seasons between 1994 and 2005.

#### Step 9: Aggregate Player Stats Per Season
Some players may have played for multiple teams in a single year. We group their stats by playerID and yearID and aggregate:
- Hits (H)
- At Bats (AB)
- Home Runs (HR)

#### Step 10: Calculate Batting Average (BA)
- We compute the Batting Average (BA) as: BA = Hits (H) / At Bats (AB)
- Missing or undefined BA values (due to division by zero) are replaced with 0.

#### Step 11: Pivot Data for Cross-Year Analysis
- We transform the data into wide format for easier comparative analysis across years:
- One column per year per metric (BA_1994, BA_1995, ..., HR_2005)

#### Step 12: Identify Mitchell Report Players
Using the mitchell_ids obtained previously from the name matching process, we label each player with a boolean flag in_mitchell_report.

#### Step 13: Prepare Long Format Data for Visualization
While the wide format is ideal for year-wise comparison, a long format version is more suitable for plotting and statistical analysis. We add the Mitchell Report flag here as well.

#### Step 14: Prepare Long Format Data for Visualization
- We visualize and summarize the offensive performance of Major League Baseball (MLB) players during the 1994–2005 period, comparing those named in the Mitchell Report against those who were not.
- The goal is to assess whether players linked to steroid use (via the Mitchell Report) exhibit different statistical distributions from their peers.
- We use a 2x2 grid of histograms to visualize the distribution of key batting statistics. This layout allows for easy side-by-side comparison between Mitchell Report players and others across multiple performance indicators

- Metric	Visual Insights:
  - Batting Average (BA) : The distributions are quite similar. Players in the Mitchell Report show a slight right shift (higher average), with most values centered around 0.26–0.27, indicating marginally better performance.
  - At Bats (AB) : Players in the Mitchell Report tend to have more plate appearances, indicating they were more active or had longer careers.
  - Hits (H) : The same trend appears — Mitchell Report players accumulated more hits on average.
  - Home Runs (HR) : There’s a heavier right tail among Mitchell Report players, suggesting higher home run output, consistent with the hypothesis of enhanced strength/performance.
- These visual cues already suggest that Mitchell Report players may outperform others in several key batting metrics.

#### Step 15: Boxplot: Home Runs by Mitchell Report Status
- Boxplots offer a concise summary of distribution characteristics such as the median, interquartile range (IQR), and outliers.
- This visualization helps identify whether Mitchell Report players hit more home runs on average or had more variability.
- A higher median or more outliers on the right could indicate exceptional performances among accused players.

#### Conclusion :

Players named in the Mitchell Report generally outperformed their peers in power-related stats (HR).

1. Home Runs by Mitchell Report Status
- Players named in the Mitchell Report (Yes) tend to have higher median home runs.
- There are also more extreme outliers (e.g., 70+ HR) among the Yes group.
- The interquartile range (IQR) is slightly wider in the Yes group, suggesting more variability.
  
Interpretation: This may suggest that players linked to PED use were able to hit more home runs, aligning with the hypothesis that steroids enhance power-hitting performance.

2. Hits by Mitchell Report Status
- Median number of hits is slightly higher for the Yes group.
- Both distributions have a large spread, and the number of hits is highly variable in both groups.
- No major outliers dominate either group, though a few high performers exist.

Interpretation: While steroids may improve strength, their effect on contact hitting (hits) is less clear. Slightly better performance for the Yes group might be due to increased playing time or better physical conditioning.

3. Batting Average by Mitchell Report Status
- Batting average medians are almost identical between the two groups.
- Both groups have similar spread and a few low-end outliers (e.g., BA below .100).
- Slightly tighter distribution in the Yes group.

Interpretation: Steroid use appears to have minimal effect on batting average, which is more influenced by technique, timing, and consistency rather than raw strength.

4. At Bats by Mitchell Report Status
- Players named in the report (Yes) had slightly more at-bats on average.
- Distribution is similar across both groups, but the Yes group shows a marginally higher median.
- Outliers extend close to 700 ABs in both groups.
 
Interpretation: This may reflect that Mitchell Report players were often more prominent and consistent starters, giving them more playing opportunities and visibility (possibly contributing to being named in the report).

#### Step 16: Summary Statistics Table
We compute summary statistics to quantify differences in key batting metrics between the two groups:
- Mean: Average performance
- Median: Midpoint of distribution
- Standard Deviation: Variability of performance
- Minimum & Maximum: Range of observed values

Across all metrics, Mitchell Report players display:
- Higher means and medians
- Greater career longevity (as implied by AB and H)

Standard deviations are quite similar, suggesting that variability in performance is consistent across both groups.

The combination of higher means, medians, and extended career metrics for players in the Mitchell Report supports the notion that they may have enjoyed enhanced performance.

While this does not establish causality, the data is consistent with the narrative that performance-enhancing drug use may have positively impacted offensive statistics.


### Problem 3
In March 2006, Major League Baseball (MLB) initiated a drug-testing program to address the widespread doping issue in professional baseball. 
Objective : 
- Comparing Batting Average (BA) and Home Runs (HR) between 2006 and 2007
- Evaluating whether the performance changes differ between players named in the Mitchell Report (suspected of doping) and those not named

#### Step 1 : Data Preparation
- We start by aggregating the batting data to compute season-level statistics for each player
- Filter data to only include 2006 and 2007:
- Pivot the dataset so that each row represents a player, and columns represent stats by year

#### Step 2: Feature Engineering
- Calculate the change in performance metrics
- Label players based on inclusion in the Mitchell Report

#### Step 3: Exploratory Data Analysis
- Distribution of Batting Average Change (2007 - 2006)
- Distribution of Home Run Change (2007 - 2006)

Interpretation
- The histogram shows the distribution of BA differences (2007 - 2006) for players.
- Blue distribution: Players named in the Mitchell Report.
- Orange distribution: Players not named in the report.
- Both distributions are approximately normal, centered around zero.

Insights
- The central peak for both groups suggests little to no overall change in batting average.
- No clear performance decline is observed among players in the Mitchell Report relative to those not in the report.
- This could indicate that the batting average was less impacted by the drug-testing program or PED usage.

Interpretation
- The histogram shows the distribution of HR differences (2007 - 2006).
- Blue = Mitchell Report players; Orange = Non-Mitchell players.
- The HR_diff distribution has heavier tails and is more skewed, indicating greater variance in HR changes.

Insights
- hile most players (both groups) hovered near zero change, there are extreme declines and gains present.
- Players in the Mitchell Report (blue line) show a wider spread, potentially suggesting more fluctuation in performance post-drug testing.
- This could imply that HR production was more sensitive to the loss of performance-enhancing drugs.

BA_diff
- Both groups experienced a drop in batting average on average from 2006 to 2007 (negative means).
- Players in the Mitchell Report had a larger drop on average (-0.0148 vs. -0.0065), though both are relatively small.
- Standard deviations are quite similar (~0.04), suggesting similar spread.
- Some players in both groups did improve their batting average (positive max values), but more players saw declines (more negative min and lower medians).
- The median in both groups is also negative, supporting that most players slightly declined.

HR_diff
- Both groups, on average, hit fewer home runs in 2007 than in 2006 (mean HR_diff is negative).
- Players in the Mitchell Report had a slightly larger average decline (-1.62 vs -1.36).
- However, the standard deviation for the Mitchell group is much larger (11.19 vs. 7.33), suggesting more variability or inconsistency.
- The range is large for both groups (from losing up to 26 HRs to gaining up to 30 HRs).
- Medians show the same trend: most players hit fewer HRs, though some did improve (positive 75% and max values).

#### Conclusion :
The implementation of MLB's drug-testing policy in 2006 did not result in a dramatic change in batting averages. However, home run totals showed more variability, especially among players named in the Mitchell Report. This suggests that power-hitting might have been more reliant on PEDs, and thus more impacted by testing policies.

### Problem 4
Objective : To compare pitching performance between two distinct eras in Major League Baseball — the Steroid Era (1994–2005) and the Recent Era (2012–2023) — using the WHIP (Walks + Hits per Inning Pitched) metric. 
- Clean and preprocess pitching data for accurate aggregation.
- Calculate WHIP for each player per year to quantify pitching efficiency.
- Analyze and visualize the distribution of WHIP across the two eras.
- Identify statistical differences in pitcher performance, which may reflect changes in player training, strategy, enforcement of drug policies, or other external factors.

#### Step 1: Data Cleaning and Preprocessing
- Initial Data Inspection : After loading the dataset, we inspect the structure of the data.
- Removing Duplicate Records : We eliminate any duplicate rows to ensure data quality
- Cleaning String Columns : To standardize string-based data, we strip whitespaces and convert text to title case

#### Step 2: Aggregating Player-Year Statistics
- To focus on yearly performance per player, we aggregate the data
- We compute innings pitched (IP) from outs recorded (since 1 inning = 3 outs)
- WHIP is a common pitching metric: WHIP = Walks (BB) + Hits (H) / Innings Pitched (IP)
- To avoid invalid WHIP values, we remove rows where innings pitched is zero

#### Step 3: Filtering by Baseball Eras
We define two distinct eras for analysis:
- Steroid Era: 1994–2005
- Recent Era: 2012–2023

#### Step 4: Descriptive Statistics
We compute basic statistical summaries for both eras:

#### Step 5: Final Cleaning Before Visualization
We ensure WHIP values are numeric and filter out any invalid entries:

#### Step 6: Visualizing WHIP Distributions
We compare the distribution of WHIP values using Kernel Density Estimation (KDE):

#### Interpretation
- The WHIP distribution for both eras shows a strong concentration between 1.0 and 2.0, with a sharp peak around 1.3–1.5.
- The Recent Era (orange line) has a slightly lower peak but is more spread out, indicating more variation in WHIP values compared to the Steroid Era.
- Despite the visual similarity, the Recent Era's curve shifts slightly to the left, suggesting marginally better average performance (lower WHIP).
- However, both eras contain a long right tail, showing the presence of pitchers with extremely high WHIP values (outliers), more so in the Recent Era.

#### Conclusion
- Pitchers in the Recent Era have slightly better WHIP performance than those in the Steroid Era, as reflected in the lower mean and median values.
- The larger standard deviation in the Recent Era suggests more inconsistency in individual performances — possibly due to increased bullpen usage or evolving pitcher roles.
- While visual differences in the KDE plot are subtle, the numerical summary confirms a small but noticeable improvement in pitching efficiency over time.
- The presence of WHIP values above 10 (which are extreme) in both eras may warrant further filtering or investigation, especially if evaluating only qualified pitchers.







