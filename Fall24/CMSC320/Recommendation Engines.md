"Given a user's history, can we predict what item they'll show interest in next?"
Examples:
- Amazon
- TikTok
- YouTube
- Facebook/Instagram (META)
## Content Based Recommendation Engines
"most simple version of making a vector to represent our collected data"
### "Featurizing" a Movie
- Title (?)
- Year
- Genre
- Critical Rating
- Director
- Actors
- Length
### Potential Issues
- A completely new user (blank slate, aka cold start).
	- Given someone you don't have any data on, how do you recommend things?
- Multiple users per account profile.
- User changing their viewing pattern (model has to retrain constantly).
- Users cannot evolve.
- Metadata is not good.
- Runs out of movies.
### Pros
- No need for other users.
- Recommend things to users with unique taste.
- Able to recommend new and unpopular items.
- Able to provide explanations.
## Collaborative Filtering Recommendation Engines
A recommendation engine that uses other people's data to give you recommendations.
- People who are similar may enjoy similar things.
After calculating similarity...
- using Euclidean or cosine distances.
We can throw it all into KNN.
- Given a user, find their K-nearest neighbors.
- Predict the user's rating based on the weighted average of those users.
### Potential Issues
- A completely new user (blank slate, aka cold start).
- Gets outside of a user's comfort zone (good and bad depending on user).
- New items--not enough data.
### Pros
- Works for any item.
- Uses more info.

# Evaluation
- We know what our true positives are, but we can't figure out false positives (if someone watched a movie outside of your recommendation engine, i.e. using another service).

|        | AntMan | Endgame | Thor 1 | Return of Thor | Iron Dude |
| ------ | ------ | ------- | ------ | -------------- | --------- |
| Justin | 1      | 0       | 0      | 1              | 0         |
How do we account for the fact that Justin may have watched Endgame or other movies elsewhere?
- Solution: We train over all of a user's related/seen items.
## Possible Evaluation Methods
- Mean Absolute Error
	- Average of the difference between the value predicted by the recommender and the actual value given by the user.
- Precision at Top Ten
	- For content based filtering, look at the highest ranked things and compute the precision of that list.
## Conclusions
- While content based filtering can produce good results, usually a combination of the two is used.
- Recommenders also take into account the overall popularity of the product.
- Recommendation engines continue to be the foundation of the monetizable internet.
