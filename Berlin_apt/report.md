<h1>How to chose Berlin locality for the relocation</h1>


<h2>Introduction/Business Problem</h2>

Work relocation is a very complicated process. One of the key questions is finding an apartment for the long-term rent.
It's very challenging to find an apartment in the unknown city in the shortest time. Especially if you don't know the local language and don't know the borough's features. 
My family is going to relocate to Berlin (Germany) in the next year. I'm going to analyze Berlin boroughs to better understand, where (boroughs and neighborhoods) I should search a flat for my family.

<b>Stakeholders/Target Audience</b>: We are not the only family which is going to relocate to Berlin, so my analysis will be helpful for all potential relocators. It allows to save the time and make the flat search easier. Code for this project is also provided, so the interested party can easily change my code for own needs.

Please note: 
1) This project is about the first stage of flat searching - choice of boroughs/neighborhoods.
2) Some information is available only on the borough level, so it will be used for aggregated conclusions.
3) The analysis includes my family preferences (flat cost, migrants situation, boroughs rating, etc), so the observations will be helpful for an average family, but the conclusions will be more suitable only for my family. 


<h2>Data</h2>

1) First of all, I need to match 96 neighborhoods with 12 boroughs of Berlin. It will be a little bit complicated because I have only separated data for each borough. 
Anyway I'll use <a href="https://en.wikipedia.org/wiki/Boroughs_and_neighborhoods_of_Berlin">wiki data</a>, and match it with the latitides/longitudes using python libraries.

2) The second dataset I'll use is Foursquare venues data with categories for each <b>neighborhood</b>. I'm going to cluster the city depending on the venues type.

3) Then I'm going to analyze each <b>borough</b> for the following criteria (each criterion is clickable to show the data):
<ul>
	<li><a href="https://www.firstcitiz.com/about-berlin/berlin-property-prices.html">Average price for 1 sq.m. of apartments</a></li>
	<li><a href="https://en.wikipedia.org/wiki/Demographics_of_Berlin">Migrants origin</a></li>
	<li><a href="https://api.placeilive.com/v1/">PlaceILive ratings information</a></li>
</ul>

Please note: Working place (or distance to it) is also should be taken to the consideration, but it's an open question for us now, so we'll ignore this factor at this stage.


<h2>Methodology</h2>

This section is divided into 4 main parts:
<ol>
	<li>Data finding, cleaning and restructuring,</li>
	<li>Exploratory analysis,</li>
	<li>Machine learning method,</li>
	<li>Visualization tools.</li>
</ol>
<h3>Data finding, cleaning and restructuring</h3>
Unfortunately, the real world isn't so perfect as study course, so I 've had some problems with the needed data finding. The easiest step was to find the list of boroughs and localities using Wikipedia. As I've mentioned above, there are 12 boroughs and 96 localities in Berlin, so the borough analysis will not be so informative, and I've decided to focus on the locality analysis. But here I've met the first limitation: most data, which I was able to find without local language was on the borough level. So I've decided to analyze localities using Foursquare API and boroughs using the other sources of information. I've tried to find average rent prices, but several years ago there was a restructure reform in Germany, so the information about rent prices was either for the old boroughs or wasn't free to download. So as a compromise, I've used average property price for 1 sq.m. as a basis of rent price differentiation.
At this stage, I've created 2 data frames with localities and boroughs data for further analysis. I've cleaned and restructured them as well. Also, I've found a geo-json file with borough information to use great visualization tool later.
 
<h3>Exploratory Analysis</h3>
I've looked through the localities data and recognized, that the majority of them is about 10 sq.km. That's why I've decided to change the radius of venues to 1500m, just to cover almost the whole locality. This decision has also a limitation, cause sometimes there are venues from the neighbor localities, but it's not a problem for our purposes. 
During the borough analysis, I've combined different factors sets, number of clusters, normalization methods and so on, and chose the most appropriate variant at the end. I've decided not to use factors, which are subjective, not enough differentiated or which were taken to the consideration at the locality level analysis. 

<h3>Machine Learning</h3>
I've decided to use K-means clustering for the problem. 
At the localities level, I was able to categorize localities into different homogenous clusters based on the venue categories. For this purposes, I've created our dataset in a format where localities should be in rows, different types of venues in columns, a specific gravity of each type of venues as values. I've done it in 2 steps. First is to create dummies, next - group the rows as needed. I've set a number of clusters to 4 due to bigger figures show not a good result.
At the boroughs level there were some problem with the results, so I've had to change the proposed clusters according to my expectations.
For our datasets, K-mean clustering's appended another column which contained the cluster number, similar sectors been grouped together.

<h3>Visualization tools</h3>
At this stage, I've used Choropleth map to show the priority of boroughs as well as mark localities clusters. 
At the locality level, I've shown all clusters, while at the final stage I've divided them into 2 groups - suitable for my family or not.


<h2>Results</h2>
On the locality levl of analysis I've obtained 4 clusters from K-means clustering:
<ul>
	<li>0 cluster - mainly center of Berlin. The most popular places are cafes and restorants.</li>
	<li>1 cluster - periphery of Berlin. The most popular places are supermarkets and tram stations. </li>
	<li>2 cluster - periphery of Berlin, but with good different places. There are supermarkets, as well as parks/lakes and cafes.<li>
	<li>3 cluster - periphery of Berlin. The most popular places are supermarket and bus stops. It's not bad zone with parks and lakes but without enough cafes and restaurants.</li>
</ul>
<img src='localities.jpg'>
According to the cluster analysis the most comfortable clusters for my family are 0 and 2. There are a lot of supermarkets, cafes and places for leisure with children.

On the borough level I've had 3 clusters I've obtained from K means clustering:
<ul>
	<li>0 cluster - good level of safety, but demographics level is average.</li>
	<li>1 cluster - average level of safety, but demographics level is enough high.</li>
	<li>2 cluster - the last group isn't convenient for us, due to there is a low level of safety, property isn't cheap.
</ul>
This clusterization hasn't fully met my expectations. So I've decided to create my own clusterization based on the above.
To the highest group (mark 3) I want to allocate indexes 3 (from 0 group), 6,9,10 (from the 1 group). It's safety group with average property prices.
Average group (mark 2) indexes 1,5 (from 0 group), 0, 7 (from 1 group). It's also a safe group, but either price is too high, or mentality index is zero.
Low group (mark 1) is the 2 group without any changes. It's not a safe group with zero mentality index.
This is the final table of boroughs clusters:
<img src='boroughs.jpg'>
Then I've combined results and here you can see the resulted map, where yellow circles on a deep green background are preferable localities for my family, the second priority is yellow circles on the light green background.
<img src='final_map.gpg'>
The final table is available in the code file.


<h2>Discussion</h2>
Due to this analysis based on my family expectations, it's not the ultimate truth. Different individuals may prefer quite periphery localities or more expensive districts. What about my analysis, it will be updated when we'll know the actual place of work. Also due to the Berlin rent market is the market of hosts, not clients, we'll have to find the appropriate flat not only according to our own expectations but proposals as well. Also, quality index information isn't the objective data, it may be helpful on the initial stage, but the real experience with the city will allow to chose the localities more wisely.


<h2>Conclusion</h2>
As demonstrated in the Result section, we can effectively simplify the flat search process by clustering the area based on the venue categories as well as analyze aggregated boroughs data. This project is more subject (in terms of results), but it can be used for other people, cause code file with each stage is also provided. This work is very useful for me both as a studying process and as a common overview of apartment searching problem during the relocation process.
