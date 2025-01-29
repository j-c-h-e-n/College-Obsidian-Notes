- Oftentimes the data we get will be messy.
	- Values will be missing.
	- Columns will need to be combined.
	- Multiple tables need to be joined.
- Our first step is cleaning.
	- Sometimes this will be simple and obvious, maybe a word is misspelled in some sort of categorical. You can fix this using apply().
	- Sometimes, the answer is non-obvious.
## Data Typing
Sometimes the data is in the wrong format.
- This will usually happen with data-times; pandas has a robust datetime library that we won't go into.
- For anything else, you can say `df['column'].astype(someType)`.
- For more complicated issues, use `df['column'.apply(conversion_function)`.
## Combining and Merging Data Sources
 Oftentimes you may need to merge various tables, making sure the file formats all match. This can be a little tricky sometimes. SANITY CHECK YOUR WORK.
## Evolving Labeling Schemes
This happens when labeled items change halfway through. 
	For example, a column that typically records "failure" is modified midway to "catastrophic failure" and "partial failure".
You'll have to cope with these as best you can:
- Divide the dataset in two?
- Infer old ratings?
## Duplicated Records
Sometimes people will put duplicate records into your data.
- If exact duplicates: `df.drop_duplicates()` will work.
- If they are duplicates with subtle differences, identify "true" values and drop the rest.
ALWAYS PERFORM THIS CHECK
## Outlier Detection
You can find outliers multiple ways. Looking for extreme z-scores is one way.
$$Z = \frac{x - \mu}{\sigma}$$
Ultimately, when doing a data science project, you have some goal in mind. Remove the outlier if it hurts that goal.
Consider:
- Is the outlier an usual event that will likely never happen again?
- Is the outlier something that will hurt your model?
- Is the outlier something that is a core element of your data?
# Missing Data
`NaN` or blanks.
- Non-responses on a survey.
- Sensor malfunction.
- Data not provided by a third party.
- Some idiot forgot to enter it.
Data can be either missing at random or influenced by certain factors. Data that is not missing at random may:
- Either: one particular value is more likely to be missing, or one particular range is more likely to be missing.
- P(Row R missing | Row R actual value) != P(Row R missing).
- Ex:
	- An entire day's worth of data got deleted. Or students with lower GPAs do not self report on a survey that asks about GPA.
## Imputation
- Median Imputation: Set the value to the median of that column
- Mode Imputation: Fill in with the most common value.
- Hot-deck imputation: Find a row that is most similar to the one with the missing value and copy that value. (best choice but a lot of work)
- Bayesian Imputation: You can literally use Bayes' Rule to come up with data.
## Missing Data in a Series
Series data is data that progresses. For example
- Water temperature sensors down a river.
- Profits over time
- A list of orders put n throughout the day by position and time.
## Imputation Using Framing Data
- IF the data is rather simple, just a simple linear interpolation will work.
## Types of Incorrect Data
- People lied.
- Instrument broke.
- You've been recording the wrong metrics.
- ...
## Detecting Incorrect Data
How do we find data that's incorrect?
- Attractors (imagine there were very few people who were 5'11", but a big spike in people who are 6').
- Discontinuities (once the limit for how much weed constituted a felony was changed, there was a huge discontinuity centered around that threshold).
- Modes that don't make sense.
- Data outside valid bounds.
- A field suddenly increases in an order of magnitude for no reason.
## Boundary Conditions
These can occur when your ability to take in data hits some sort of artificial cap.
	Your instruments have a maximum measurement.
	Your database caps certain measures.
	Your survey values only go up to a certain amount.
	