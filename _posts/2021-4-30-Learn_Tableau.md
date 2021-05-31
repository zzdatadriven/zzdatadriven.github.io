---
title: "Learn Tableau"
tags: [Tableau]
style: border
color: primary
description: Learn Tableau concepts and understand the idea behind.    
---
 **Outline**
* [Connect and Prepare Data]()
* [Build Visualizations](#build-visualizations)

  * [Group Your Data](#group-your-data)
  * [Create Sets](#create-sets)
  * [Create Parameters](#create-parameters)
  * [Filter and Sort Data](#filter-and-sort-data)
    
* [Analyze Your Data]()
* [Maps and Geographics Data]()
* [Present Dashboards and Stories]()

------------------------------------------------------------------------------------------

# **Build Visualizations**

## Group Your Data  

**Why?** for correcting data errors (e.g. combining CA, Cali, and California) and answering what if type questions (e.g. 'what if we combined the east and west regions') 

**How?** multiple ways to create a group. 1. create a group from a field in the data pane; 2. select data points in the view and click the group icon. [[video tutorial]](https://www.tableau.com/learn/tutorials/on-demand/grouping?playlist=230853)

Include an Other Group. This is useful for highlighting certain groups or comparing specific groups against everything. 

Additional ways to group. use calculation fields and parameters to create group with if&then functions to group members under certain conditions. [[video tutorial]](https://www.tableau.com/learn/tutorials/on-demand/additional-ways-group?playlist=230853)

## Create Sets

Sets are custom fields that define a subset of data based on some conditions. There are two types of sets: dynamic sets and fixed set.

**sets actions**

**set control** 

#### dynamic set 

The members of a dynamic set change when underlying data changes. 

Dynamic sets can only be based on a single dimension. 

#### fixed set 

The members of a fixed set do not change even if the underlying data changes. A fixed set can be based on a single dimension or multiple dimensions.  

#### Combine sets 

#### Sets for Top N and Others

## Create Parameters

A parameter is a workbook variable such as a number, date, or string that can replace a constant value in a **calculation, filter, or reference line**. [[video tutorial]](https://help.tableau.com/current/pro/desktop/en-us/parameters_create.htm)

## Filter and Sort Data

## [Choose the right chart type for your data](https://help.tableau.com/current/pro/desktop/en-us/what_chart_example.htm)

Understanding what you want will help determine how you want to show your visualizations. Therefore, it is important to know what different charts can show. 

#### Change Over time

Line chart, slope chart, highlight tables. 

#### Correlation

Scatter plot, highlight tables, heat maps. 

#### Magnitude

Bar chart, packed bubble chart, line chart. 

#### Deviation 

Deviation chart show how far a value is from some baseline, such as average, median. 

Bullet chart, bar chart, and combination chart. 

#### Distribution 

Histogram, population pyramids, Pareto chart, box chart. 

#### Ranking

Bar chart that integrate rank calculations, top n sets, or key progress indicators. 

[table calculation](https://help.tableau.com/current/pro/desktop/en-us/calculations_tablecalculations_definebasic_runningtotal.htm) 

#### Part to whole 

Pie chart, area chart, stacked bar chart, treemaps. 

#### Spatial

filled maps, point distribution maps, symbol maps, density maps. 

#### Flow

Sankey diagrams



