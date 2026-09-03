This project set out to identify the most 'unfortunate' NFL franchise based on their Super Bowl performance. Here's a detailed breakdown of what was done:

Data Loading and Initial Exploration:

The project began by mounting Google Drive to access the SuperBowl_History.csv dataset, which contains records of all Super Bowl games.
The CSV was loaded into a pandas DataFrame, and pd.set_option("display.max_columns", None) was used to ensure all columns were visible during initial inspection.
A copy of the original DataFrame (df) was made (new_df = df.copy()) to preserve the original data while allowing for modifications.
Data Cleaning and Feature Engineering:

A new column, HoF (Hall of Fame), was created to identify MVPs who are likely in the Hall of Fame, based on a '+' suffix in the MVP column. This column was then moved to a more prominent position in the DataFrame.
The Super Bowl number (SBnum) was extracted from the SB column (e.g., 'LIX (59)' becomes '59'). Regular expressions were used to remove non-numeric characters, and the column was converted to an integer type. This SBnum column was also inserted into the DataFrame for easier chronological analysis.
For the core analysis, a new DataFrame (new_df) was created containing only the Loser, LosePts, Date, SB, and Winner columns, as these were deemed most relevant to the question of team misfortune.
Analyzing Wins and Losses:

value_counts() was used on both the Loser and Winner columns to get a count of Super Bowl appearances and victories/defeats for each team.
These two series were then concatenated and merged into a single DataFrame (new_df) to show total losses and total wins for each team. fillna(0) was applied to handle teams that only appeared in one list (e.g., only lost, never won) and convert counts to integers.
Visualizing Losses (Initial Attempt):

A scatter plot was generated to visualize the Points Lost by each Losing Team in Super Bowls. This involved mapping team names to numeric values for plotting.
The purpose was to visually identify teams that frequently lost by narrow margins or by large deficits. While informative, it was determined that a more precise metric was needed.
Defining and Calculating the 'Heartbreak Score':

The concept of a 'Heartbreak Score' was introduced, focusing on teams that reached the Super Bowl but consistently fell short, especially in 'close losses' (losing by 10 points or fewer).
Teams that had won a Super Bowl were excluded from this analysis, as the focus was strictly on the 'most unfortunate' teams that had never achieved victory.
close_losses_count was calculated by filtering the original DataFrame for LosePts <= 10 and then counting the losing teams.
The Heartbreak_Score was calculated as (close_losses / total_losses) * 100. To prevent division by zero errors for teams with no losses, replace(0, np.nan) was used, and fillna(0) was applied at the end to set the score to zero for teams with no close losses or total losses.
A temporary DataFrame for close_losses was created and merged with new_df to ensure all necessary data was present for the calculation.
The final new_df was filtered to include only teams with wins == 0 and then sorted by Heartbreak_Score in descending order.
Findings and Historical Context:

The Heartbreak Score analysis revealed the Minnesota Vikings as the team with the highest score (75%), having lost 3 out of their 4 Super Bowl appearances by 10 points or less, and never winning. The Buffalo Bills, despite their four consecutive losses, had a Heartbreak Score of 0% because none of their losses were by 10 points or fewer.
Further research was then conducted into the Minnesota Vikings' Super Bowl history to provide additional context, including potential off-field issues (like segregation for opponents in Super Bowl IV), subpar facilities (Super Bowl VIII), and game-related controversies (Super Bowl IX). This detailed examination underscored their historical 'unfortunate' status.
