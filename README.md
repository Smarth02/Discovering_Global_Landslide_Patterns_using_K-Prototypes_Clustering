<h1>Discovering Global Landslide Patterns with K-Prototypes Clustering</h1>
<p>Landslides are not all the same. A slow slide above a road in the US and a deadly rain-triggered slide in South Asia are very different events, but they sit in the same table.<br>
This project asks a simple question: if we let an algorithm group landslides by itself, what natural patterns show up?
</p>
<h2>The data</h2>
<p>
  I used the Global Landslide Catalog, a public record of landslide events from around the world.
  <ul>
<li>11,033 events in the raw file, 32 columns
<li>9,393 events left after cleaning
<li>A mix of numbers (fatalities, latitude, longitude, location accuracy) and labels (landslide type, trigger, size, setting, country)</ul>
That mix is the main challenge. Most clustering tools handle only one kind of data well.

</p>
<h2>Why K-Prototypes?</h2>
<p>K-Means works with numbers only. K-Modes works with categories only. K-Prototypes combines both: it measures distance on numbers the usual way, and on categories by counting how many values do not match.<br>
Since landslide data has both, this was the natural fit.</p>
<h2>How I did it</h2>
<ol>
<li>Cleaning
<ul><li>Turned event_date into a proper date and time format
<li>Converted location_accuracy from text like "5km" and "exact" into real numbers
<li>Filled missing numeric values (location accuracy, fatalities) with the median
<li>Marked missing size and setting as "Unknown" instead of throwing those rows away
<li>Dropped rows with no country, no administrative region, or no closest place name
<li>Dropped injury_count, because more than half of it was empty
<li>Checked for duplicates (none) and negative values (none)
</ul>
  <li>Preparing features<ul>
<li>Numeric: location_accuracy(km), fatality_count, longitude, latitude, scaled with StandardScaler
<li>Categorical: landslide_category, landslide_trigger, landslide_size, landslide_setting, country_name, country_code
  </ul>
  <li>Choosing the number of clusters
<p>I ran K-Prototypes for 2 to 10 clusters and plotted the cost for each (the elbow method). I also read the profile of every run, not just the curve, and settled on 6 clusters.</p>
<li>Reading the clusters<p>
For each cluster I looked at the average of every numeric feature and the share of every category.</p>
</ol>
<h2>Key Learnings</h2>
<ul>
<li>Applying unsupervised learning to real-world datasets involves more than fitting a model.

<li>Mixed numerical and categorical data require appropriate distance and clustering methods.

<li>A lower clustering cost does not necessarily imply better-separated clusters.

<li>Model stability and cluster interpretability are important alongside numerical evaluation metrics.

<li>Gower distance provides a way to evaluate similarity when features have different data types.</ul>
<h2>Limitations</h2>
<ul>
  <li>Clusters come from the data, not from geology. They describe how events were recorded, and recording quality differs by country.
<li>Filling missing values with the median is simple, but it can hide real variation.
<li>Country name and country code carry the same information, so geography may count for more than I intended.
  <li>Results can shift a little between runs, because K-Prototypes starts from random centers.
</ul>
<h2>What I would do next</h2>
<ul>
  <li>Test different weights between numeric and categorical features (the gamma parameter)
    <li>Add a rainfall or elevation dataset to explain the clusters
    <li>Plot the clusters on a world map
    <li>Compare against other methods such as DBSCAN or Gower distance with hierarchical clustering
</ul>
<h2>Tech Stack</h2>
<p>Python, pandas, NumPy, scikit-learn, kmodes, matplotlib, seaborn</p>
<h2>Conclusion</h2>
<p>This project explores how K-Prototypes can be applied to mixed-type landslide data to discover groups of events with similar characteristics.<br>

The main takeaway is that meaningful clustering requires more than choosing a model: preprocessing, evaluation, stability, and interpretation all play an important role in understanding the results.</p>
