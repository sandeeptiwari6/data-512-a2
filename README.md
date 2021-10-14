# A2: Bias in Data Assignment
___

## Goal
___
The goal of this assignment is to explore the concept of bias through data on Wikipedia articles - specifically, articles on political figures from a variety of countries. For this assignment, I combined a dataset of Wikipedia articles with a dataset of country populations, and used a machine learning service called ORES to estimate the quality of each article.

## License + API Documentation
___
The Wikipedia politicians by country dataset can be found on [Figshare](https://figshare.com/articles/dataset/Untitled_Item/5513449). Read through the documentation for this repository, then download and unzip it to extract the data file, which is called `page_data.csv`.

Something to note here is that some instances in `page_data.csv` contain some page names that start with the string "Template:". These pages are not Wikipedia articles.

The population data is available in CSV format as [`WPDS_2020_data.csv`](https://docs.google.com/spreadsheets/d/1CFJO2zna2No5KqNm9rPK5PCACoXKzb-nycJFhV689Iw/edit). This dataset is drawn from the [world population data sheet](https://www.prb.org/international/indicator/population/table/) published by the Population Reference Bureau.

Another thing to note is that `WPDS_2020_data.csv` contains some rows that provide cumulative regional population counts, rather than country-level counts. These rows are distinguished by having ALL CAPS values in the 'geography' field (e.g. AFRICA, OCEANIA). These rows won't match the country values in page_data.csv, but you will want to retain them (either in the original file, or a separate file) so that you can report coverage and quality by region in the analysis section.

## Using ORES to obtain Article Quality Predictions
___
Article quality data was obtained from a machine learning system called ORES. This was originally an acronym for "Objective Revision Evaluation Service" but was simply renamed “ORES”. ORES is a machine learning tool that can provide estimates of Wikipedia article quality. The article quality estimates are, from best to worst:

1. FA - Featured article
2. GA - Good article
3. B - B-class article
4. C - C-class article
5. Start - Start-class article
6. Stub - Stub-class article

You can obtain this data using either the ORES client or ORES REST API. The documentation for the latter can be found [here](https://ores.wikimedia.org/v3/#!/scoring/get_v3_scores_context_revid_model).

NOTE: I had extreme difficulty using the client when trying to `pip install` the ores library, which led me to use the REST API instead.

## Data Overview
___
After combining all these datasets together, I pushed the resultant datasets to the `data_clean` folder.

I removed any rows that do not have matching data, and output them to a CSV file called:
  `wp_wpds_countries-no_match.csv`
  
I then consolidated the remaining data into a single CSV file called:
`wp_wpds_politicians_by_country.csv`

The schema for that file should look something like this:

| Column               |
|----------------------|
| country              |
| article_name         |
| revision_id          |
| article_quality_est. |
| population           |

Note: revision_id here is the same thing as rev_id, which you used to get scores from ORES.

## Reflection
___
Overall, I was extremely surprised by how many articles there were relative to populations of countries I had barely even heard of. I would have suspected there to be more articles about politicians in the largest countries. Furthermore, I also expected there to generally be higher quality articles in these larger countries, simply because of there being more people able to contribute to these articles, particularly in their respective languages. This hypothesized bias was more or less supported by the results. There were also very many articles that simply were not able to be collected using the REST API, and were simply thrown out. I didn't do further investigation to see if there was any systematic trend for why these articles were unable to be accessed, but it might be useful to see if there was any other potential biases that were introduced using this methodology.

However, we are collecting data specifically from the English side of Wikipedia, so there might be fewer people that are able to contribute to the English versions of articles in countries that might speak more obscure languages. This is why we might find fewer high-quality articles in English about politicians in these smaller countries, simply because of the general language barrier, and naturally introduces a bias against these articles. This becomes extremely evident when we sorted the countries by high-quality article percentages, as most of these countries are those that have a large populations and general global presence as well.

We might be able to fix these issues and biases if we were to simply remove the restriction of only using English Wikipedia. We could collect all data accessible to the API across all available languages, and append it to the data we have already collected. This would at least remove any biases against some of the countries' articles that were not deemed high-quality because it was not properly fleshed out in English. Some potential issues with this approach is the sheer volume of data that we would need to be able to store as we are now collecting data across many other nations. But it would at least (more-or-less) correct for the aforementioned biases/limitations.
