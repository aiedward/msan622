Final Project
==============================

| **Name**  | Yosuke Katada  |
|----------:|:-------------|
| **Email** | ykatada@dons.usfca.edu |

## Instruction ##
Before you start shiny app, please make sure that you install the following packages:
- `ggplot2`
- `reshape2` 
- `reshape` 
- `scales` 
- `grid` 
- `plyr` 
- `RColorBrewer` 

After that, use the following code to run this `shiny` app:

```
library(ggplot2)
library(reshape2)
library(reshape)
library(scales)
library(grid)
library(plyr)
library(RColorBrewer)
library(shiny)
runGitHub("msan622", "yosukekatada", subdir = "final-project")
```

## Discussion ##

### Dataset ###
The dataset is about the customers' response to direct marketing campaigns (phone call) to  make them subscribe term deposit accounts at a Portuguese banking institution. The data has 45211 rows and 17 variables. Each row represents each customer. The independent variables consist of demographic, transaction-based and historical marketing reactions. A dependent variable is "has the client subscribed a term deposit?", which is binary response. 

At the beginning,  I changed the order of factor levels about the binary response, ("No", "Yes) , regarding whether a customer accepted the campaign for visualization. The original order of the factor levels was "No" and "Yes". However, the audiences seem to be interested in how many or much percentage of customers accepted the campaign. So, I changed the order of the factor levels from ("No", "Yes") to ("Yes", "No").

The following is the first 10 data points.

![IMAGE](img/dataset.png)

[Data Source is here.] (http://archive.ics.uci.edu/ml/datasets/Bank+Marketing)

#### Introduction ####
I suppose that this shiny app tries to help a marketing analytics personnel to analyze the data, get the insights, and finally evaluate ROI of the marketing campaign. This application consists of four components: Univariate plots, Multivariate plots, Modeling, and ROI simulation. 

The following is the summary about how each component associates with techniques and interactivity.

| **Component**  | **Techniques**  | **Interactivity**  |
|:---------------|:----------------|:--------------------|
| Basic Profiling (Univariate plots) | Density plot / Box plot / Bar chart / Stacked Bar chart (Normalized) | Switch to small multiple |
| Multivariate Plots | Heat Map / Scatte Plot | Zooming |
| Modeling | Bar Chart | Sorting |
| ROI Simulation | Line Chart / Bar Chart | None |


### Techniques ###

#### Basic Profiling (Univariate plots) ####

##### Density plot and Box plot #####
In the dataset, there are seven numeric variables such as `age`,`balance`, and so on. For them, I created density plot and box plot. Among them, `duration`, which means last contact duration (I guess how many seconds a customer talked with the bank personnel at the most recent call), has distinct distributions between the customer who subscribed term deposit and the customer who did not.  

![IMAGE](img/density_box.png)

- Lie Factor: Almost one
- Data Ink Ratio: High enough for easily understanding
- Data Density: It is possible that the two distributions are almost same shape. Therefore, to avoid data density problem, I used small multiple.


##### Bar chart and Stacked Bar chart#####
There are lots of categorical variables such as `job`,`marital`, and so on. For categorical variables, bar chart is one of the easiest visualization to demonstrate how many customers or how much percentage of customers positively reacted to the campaigns. For example, the following is the bar chart and the normalized stacked bar chart about `job`. By looking at the normal bar chart, I understood which jobs were dominant in the data. Also, when I looked at normalized stacked bar chart, student and retired customer accepted the campaign more likely than other jobs.

![IMAGE](img/bar.png)
![IMAGE](img/100bar.png)

- Lie Factor: The x axis is  fixed from 0 to the highest. So, Lie factor should be one.
- Data Ink Ratio: High enough because I deleted minor grids and customized major grid.
- Data Density: I flipped x and y axis for the audiences to read the labels more easily. Therefore, data density is fair. 

#### Multivariate Plots ####
##### Heat Map #####
Although I found out some interesting variables in the univariate plots, I needed deeper analysis. So, at this time, multivariate plots were useful. First of all, heat map helped me to examine the relationship between two variables. For example, when I used `duration` as x axis and `job` as y axis for heat map, I found out some buckets which have higher percentage of the customer who subscribed term deposit (Red buckets).

![IMAGE](img/heatmap.png)

- Lie Factor: To avoid exaggeration, I used equal interval method instead equal frequency metohd for discretization. So, I believe that there is no lie factor.
- Data Ink Ratio: High enough
- Data Density: Although some one plots the numbers on the heat map, I did not do so because I wanted to avoid data density problem. 


##### Scatter Plot #####
Heat map helped me to compare any two variables if I discretized numeric variables. However, I am more interested in the distribution and relationship only for numeric variables. So, I created a scatter plot. 

![IMAGE](img/scatterplot.png)

- Lie Factor: Almost one because I did not emphasize any elements.
- Data Ink Ratio: High enough
- Data Density: Data density may be a litte bit problem since the data points are more than 40,000 in the data. So, in order to ease too dense points, I did random sampling, employed facet_wrap function, and specified smaller alpha for points.

#### Modeling ####
##### Bar Chart #####
For predicting the customers' response, I developed logistic regression model. In order to evaluate which variables have impact on the prediction, I created a bar chart to visualize the standardized coefficients. The variables were sorted by decreasing order of coefficients. According to the bar chart,  as `duration` increased,  the likelihood that the customer subscribed term deposit significantly increased. On the other hand, if the contact was unknown, the likelihood decreased most significantly. 

![IMAGE](img/coef.png)

- Lie Factor: Almost one because I did not emphasize any elements.
- Data Ink Ratio: High enough
- Data Density: Labels is the most important things for the audience. So, I fliped x and y axis (if I used labels in x axis, some labels are overlapped each other). I think that data density does not have issues. 



#### ROI Simulation ####
##### Multiple Line Plot #####
So far, I analyzed the data to investigate which variables are influential to determine what customers more likely subscribed term deposit by a marketing campaign. However, more important point is to translate this analytical result into a business context. Therefore, the final part is ROI simulation. Based on the prediction model, I simulated ROI by assuming that the bank executes the similar marketing campaign. For communication purpose, I used the simplest plots like a multi-line chart and a bar chart because the target audience would be non-experts. 
The multiple line chart represents the simulated revenue and cost along projected time line. Also, the bar chart at the bottom left is profit (= Revenue-Cost at the previous line chart). The third plot at the bottom right is a simple bar chart to compare the inflow(or outflow) of cash caused by a marketing campaign and the execution cost associated with the campaign.

![IMAGE](img/roi.png)

- Lie Factor: Almost one
- Data Ink Ratio: High enough since I deleted unnecessary grids.
- Data Density: I beleive that there is no issues on data density.


### Interactivity ###
#### Basic Profiling (Univariate plots) ####
In "Basic Profiling" page, you can select any variable. Shiny app automatically show you a bar chart or a density plot with box plots based on whether the selected variable is numeric or categorical. When you select a numeric variable, you can use log scale and small multiple for density plot. In case that you choose a categorical variable, you can switch between a simple bar chart and normalized stacked bar chart. Also you can sort the bars by decreasing or increasing order. 

![IMAGE](img/basicprofiling.png)


#### Heat Map (Multivariate Plots) ####
Like the previous page, you can choose two variables for constructing heat map. If you choose a numeric variable, you can specify the bin of discretization. This means you can zoom in or zoom out as you like. 

![IMAGE](img/heatmap_ui.png)


#### Scatter Plot (Multivariate Plots) ####
As another multivariate plot, you can look at a scatter plot. In this scatter plot, you can see the detail as well as the overall. Also, this plot is dense. So you can select the random sampling portion of all the data points for constructing the plot. In addition, you can select dot size and alpha.

![IMAGE](img/scatterplot_ui.png)


#### Modeling ####
In this part, most of interactivity is associated with modeling. For example, you can select which independent variables are included in the modeling. In terms of data visualization, you can select either coefficients or p value for sort key and choose sort order.

![IMAGE](img/logisticreg_ui.png)

If you clicked a tab called "Standardized Coefficients", you can see the following image.
![IMAGE](img/logisticreg_ui2.png)

#### ROI Simulation ####
ROI simulation page only have the numeric input to simulate ROI. For example, you can input the assumed cost per person to execute a marketing campagin.

![IMAGE](img/roi_ui.png)


### Prototype Feedback ###
- The prototype was almost same as the final deliverable other than the following changes. I got very helpful feedback, and I reflected them to my work.

| **Component** | **Before**  | **Feedback**  | **changes**  |
|:-------------------|:---------------|:----------------|:--------------------|
| Heat Map |Background color in heat map was **black** |  "Black color is too dominant" | I changed the color from **black to dark gray** |
| Modeling | Standardized coefficients bar chart could be **sorted by coeffcient** |"The bar plot could be sorted by p-value as well as by the coefficients" | I added **p value as another sort key**|
| ROI Simulation |In "profit/loss bar chart", the bar's color was **always blue** | "It is cool if you can distinguish profit and loss by different colors" | I used **red for loss and blue for profit** |

- All the feedback above were helpful because I thought that they improved graphs' visibility and the application's usability, and therefore I reflected them to my application. Especially, I did not expect that the background color of the heat map was important for audience. However, the group members pointed out that black color distracted the audiences' focus. This feedback was beyond my imagination.

- I got a feedback that I would plot the data points on a scatter plot and overlay a fitted model to the scatter plot. Actually, I already implemented this plot (you can see `get_DecisionBoundary` function in `get_plots.R`), the plot was too dense. As I discussed in scatter plot section, the data has more than 40,000 data points, and even a simple scatter plot easily gets to have data density problem. So, I gave up it, although I appreciated this feedback. 

### Challenges ###
- One of the difficult parts was **choosing colors in heat map**. In terms of overall visualizations in this app, I used red for "Yes" and blue for "No". However, I needed to decide the middle color because heat map needed gradation. I tried many times until the middle color looks reasonable. 

- Another challenge was the thing that **the simpler UI needed much programming efforts**, especially in the first page called "Basic Profiling". This page looks simple, but the inputs are changed based on the type of the selected variable. Also, the chart itself is switched between bar chart and density plot with box plots. 