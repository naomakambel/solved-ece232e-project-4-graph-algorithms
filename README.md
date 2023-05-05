Download Link: https://assignmentchef.com/product/solved-ece232e-project-4-graph-algorithms
<br>
<h1></h1>

In this project we will explore graph theory theorems and algorithms, by applying them on real data. In the first part of the project, we consider a particular graph modeling correlations between stock price time series. In the second part, we analyse traffic data on a dataset provided by Uber.

<h1>1. Stock Market</h1>

In this part of the project, we study data from stock market. The data is available on this <a href="https://www.dropbox.com/s/83l60htndqpn3fv/finance_data.zip?dl=0">Dropbox Link</a><a href="https://www.dropbox.com/s/83l60htndqpn3fv/finance_data.zip?dl=0">.</a> The goal of this part is to study correlation structures among fluctuation patterns of stock prices using tools from graph theory. The intuition is that investors will have similar strategies of investment for stocks that are effected by the same economic factors. For example, the stocks belonging to the transportation sector may have different absolute prices, but if for example fuel prices change or are expected to change significantly in the near future, then you would expect the investors to buy or sell all stocks similarly and maximize their returns. Towards that goal, we construct different graphs based on similarities among the time series of returns on different stocks at different time scales (day vs a week). Then, we study properties of such graphs. The data is obtained from Yahoo Finance website for 3 years. You’re provided with a number of csv tables, each containing several fields: Date, Open, High, Low, Close, Volume, and Adj Close price. The files are named according to <em>Ticker Symbol </em>of each stock. You may find the market sector for each company in Name sector.csv.

<h2>1. Return correlation</h2>

In this part of the project, we will compute the correlation among log-normalized stock-return time series data. Before giving the expression for correlation, we introduce the following notation:

<ul>

 <li><em>p<sub>i</sub></em>(<em>t</em>) is the closing price of stock <em>i </em>at the <em>t<sup>th </sup></em>day</li>

 <li><em>q<sub>i</sub></em>(<em>t</em>) is the return of stock <em>i </em>over a period of [<em>t </em>− 1<em>,t</em>]</li>

 <li><em>r<sub>i</sub></em>(<em>t</em>) is the log-normalized return stock <em>i </em>over a period of [<em>t </em>− 1<em>,t</em>]</li>

</ul>

<em>r<sub>i</sub></em>(<em>t</em>) = log(1 + <em>q<sub>i</sub></em>(<em>t</em>))

Then with the above notation, we define the correlation between the log-normalized stock-return time series data of stocks <em>i </em>and <em>j </em>as

where h·i is a temporal average on the investigated time regime (for our data set it is over 3 years).

<strong>QUESTION 1: </strong>What are upper and lower bounds on <em>ρ<sub>ij</sub></em>? Provide a justification for using lognormalized return (<em>r<sub>i</sub></em>(<em>t</em>)) instead of regular return (<em>q<sub>i</sub></em>(<em>t</em>)).

<h2>2. Constructing correlation graphs</h2>

In this part,we construct a correlation graph using the correlation coefficient computed in the previous section. The correlation graph has the stocks as the nodes and the edge weights are given by the following expression

q

<em>w<sub>ij </sub></em>=       2(1 − <em>ρ<sub>ij</sub></em>)

Compute the edge weights using the above expression and construct the correlation graph.

<strong>QUESTION 2: </strong>Plot a histogram showing the un-normalized distribution of edge weights.

<h2>3. Minimum spanning tree (MST)</h2>

In this part of the project, we will extract the MST of the correlation graph and interpret it.

<strong>QUESTION 3: </strong>Extract the MST of the correlation graph. Each stock can be categorized into a sector, which can be found in Name sector.csv file. Plot the MST and color-code the nodes based on sectors. Do you see any pattern in the MST? The structures that you find in MST are called Vine clusters. Provide a detailed explanation about the pattern you observe.

<h2>4. Sector clustering in MST’s</h2>

In this part, we want to predict the market sector of an unknown stock. We will explore two methods for performing the task. In order to evaluate the performance of the methods we define the following metric

where <em>S<sub>i </sub></em>is the sector of node <em>i</em>. Define

where <em>Q<sub>i </sub></em>is the set of neighbors of node <em>i </em>that belong to the same sector as node <em>i </em>and <em>N<sub>i </sub></em>is the set of neighbors of node <em>i</em>. Compare <em>α </em>with the case where

<strong>QUESTION 4: </strong>Report the value of <em>α </em>for the above two cases and provide an interpretation for the difference.

<h2>5. Correlation graphs for weekly data</h2>

In the previous parts, we constructed the correlation graph based on daily data. In this part of the project, we will construct a correlation graph based on weekly data. To create the graph, sample the stock data weekly on Mondays and then calculate <em>ρ<sub>ij </sub></em>using the sampled data. If there is a holiday on a Monday, we ignore that week. Create the correlation graph based on weekly data.

<strong>QUESTION 5: </strong>Extract the MST from the correlation graph based on weekly data. Compare the pattern of this MST with the pattern of the MST found in Question 3.

<ol start="2">

 <li><strong> Let’s Help Santa!</strong></li>

</ol>

Companies like Google and Uber have a vast amount of statistics about transportation dynamics. Santa has decided to use network theory to facilitate his gift delivery for the next Christmas. When we learned about his decision, we designed this part of the project to help him. We will send him your results for this part!

<h2>1. Download the Data</h2>

Go to <a href="https://movement.uber.com/">“Uber Movement”</a> website and download data of <strong>Travel Times by Month (All Days), 2019 Quarter 4</strong>, for Los Angeles area<a href="#_ftn1" name="_ftnref1"><sup>[1]</sup></a>. The dataset contains pairwise traveling time statistics between most pairs of points in the Los Angeles area. Points on the map are represented by unique IDs. To understand the correspondence between map IDs and areas, download <strong>Geo Boundaries </strong>file from the same website<a href="#_ftn2" name="_ftnref2"><sup>[2]</sup></a>. This file contains latitudes and longitudes of the corners of the polygons circumscribing each area. To be specific, if an area is represented by a polygon with 5 corners, then you have a 5×2 matrix of the latitudes and longitudes, each row of which represents latitude and longitude of one corner.

We strongly suggest using igraph library in Python language for this section. Most data processing steps will become far more convenient if you use Python instead of R.

<h2>2. Build Your Graph</h2>

Read the dataset at hand, and build a graph in which nodes correspond to locations, and undirected weighted edges correspond to the mean traveling times between each pair of locations (<strong>only December</strong>). Add the mean of the coordinates of each region’s polygon corners (a 2-D vector) as an attribute to the corresponding vertex.

The graph will contain some isolated nodes (extra nodes existing in the Geo Boundaries JSON file) and a few small connected components. Remove such nodes and just keep the largest connected component of the graph. In addition, merge duplicate edges by averaging their weights <a href="#_ftn3" name="_ftnref3"><sup>[3]</sup></a>. We will refer to this cleaned graph as <em>G </em>afterwards.

<strong>QUESTION 6: </strong>Report the number of nodes and edges in <em>G</em>.

<h2>3. Traveling Salesman Problem</h2>

<strong>QUESTION 7: </strong>Build a minimum spanning tree (MST) of graph <em>G</em>. Report the street addresses of the two endpoints of a few edges. Are the results intuitive?

<strong>QUESTION 8: </strong>Determine what percentage of triangles in the graph (sets of 3 points on the map) satisfy the triangle inequality. You do not need to inspect all triangles, you can just estimate by random sampling of 1000 triangles.

Now, we want to find an approximation solution for the traveling salesman problem (TSP) on <em>G</em>. Apply the 1-approximate algorithm described in the class<a href="#_ftn4" name="_ftnref4"><sup>[4]</sup></a>. Inspect the sequence of street addresses visited on the map and see if the results are intuitive.

<strong>QUESTION 9: </strong>Find an upper bound on the empirical performance of the approximate algorithm:

Approximate TSP Cost

<em>ρ </em>=

Optimal TSP Cost

<strong>QUESTION 10: </strong>Plot the trajectory that Santa has to travel!

<h2>4. Analysing Traffic Flow</h2>

Next December, there is going to be a large group of visitors travelling between a location near Malibu to a location near Long Beach. We would like to analyse the maximum traffic that can flow between the two locations.

<h2>5. Estimate the Roads</h2>

We want to estimate the map of roads without using actual road datasets. Educate yourself about <em>Delaunay triangulation </em>algorithm and then apply it to the nodes coordinates<a href="#_ftn5" name="_ftnref5"><sup>[5]</sup></a>.

<strong>QUESTION 11: </strong>Plot the road mesh that you obtain and explain the result. Create a graph <em>G</em><sub>∆ </sub>whose nodes are different locations and its edges are produced by triangulation.

<h2>6. Calculate Road Traffic Flows</h2>

<strong>QUESTION 12: </strong>Using simple math, calculate the traffic flow for each road in terms of cars/hour. Report your derivation.

Hint: Consider the following assumptions:

<ul>

 <li>Each degree of latitude and longitude ≈ 69 miles</li>

 <li>Car length ≈ 5 m = 0.003 mile</li>

 <li>Cars maintain a safety distance of 2 seconds to the next car</li>

 <li>Each road has 2 lanes in each direction</li>

</ul>

Assuming no traffic jam, consider the calculated traffic flow as the max capacity of each road.

<h2>7. Calculate Max Flow</h2>

Consider the following locations in terms of latitude and longitude:

<ul>

 <li>Source coordinates: [34.04, -118.56]</li>

 <li>Destination coordinates: [33.77, -118.18]</li>

</ul>

<strong>QUESTION 13: </strong>Calculate the maximum number of cars that can commute per hour from Malibu to Long Beach. Also calculate the number of edge-disjoint paths between the two spots. Does the number of edge-disjoint paths match what you see on your road map?

<h2>8. Prune Your Graph</h2>

In <em>G</em><sub>∆</sub>, there are a number of unreal roads that could be removed. For instance, you might notice some unreal links along the concavities of the beach, as well as in the hills of Topanga. Apply a threshold on the travel time of the roads in <em>G</em><sub>∆ </sub>to remove the fake edges. Call the resulting graph <em>G</em><sup>˜</sup><sub>∆</sub>.

<strong>QUESTION 14: </strong>Plot <em>G</em><sup>˜</sup><sub>∆ </sub>on actual coordinates. Do you think the thresholding method worked?

<strong>QUESTION 15: </strong>Now, repeat question 13 for <em>G</em><sup>˜</sup><sub>∆ </sub>and report the results. Do you see any changes? Why?

<h2>9. Define Your Own Task</h2>

Brainstorm and define a new interesting task based on the dataset at hand. This part is openended and you’ll be graded based on your creativity. You may also wish to go beyond, and use any other navigation- / traffic- related dataset and define your favorite task. This part is worth 20% credit of the project. You may discuss your ideas with the TA in designated office hours.

<a href="#_ftnref1" name="_ftn1">[1]</a> If you download the dataset correctly, it should be named as los angeles-censustracts-2019-4-All-MonthlyAggregate.csv

<a href="#_ftnref2" name="_ftn2">[2]</a> The file should be named los angeles censustracts.json

<a href="#_ftnref3" name="_ftn3">[3]</a> Duplicate edges may exist when the dataset provides you with the statistic of a road in both directions. We remove duplicate edges for the sake of simplicity.

<a href="#_ftnref4" name="_ftn4">[4]</a> You can find the algorithm in: <a href="https://www.dropbox.com/s/0e5iw5w8d75m19z/book-scrap.pdf?dl=0">Papadimitriou and Steiglitz,</a> <a href="https://www.dropbox.com/s/0e5iw5w8d75m19z/book-scrap.pdf?dl=0"><em>“Combinatorial optimization: algorithms and complex</em></a><a href="https://www.dropbox.com/s/0e5iw5w8d75m19z/book-scrap.pdf?dl=0"><em>ity”</em></a><a href="https://www.dropbox.com/s/0e5iw5w8d75m19z/book-scrap.pdf?dl=0">, Chapter 17, page 414</a>

<a href="#_ftnref5" name="_ftn5">[5]</a> You can use scipy.spatial.Delaunay in Python