---
title: Glossary
sidebar_label: Glossary
copyright: (C) 2007-2018 GoodData Corporation
id: version-4.1.1-glossary
original_id: glossary
---

## A

**AFM \(Attribute - Filter - Measure\)** is unified input for creating a visualization using the GoodData UI SDK.AFM is a combination of attributes, measures and filters that describes a query you want to execute. In terms of underlying API, it is similar to when you are creating an insight using [Analytic Designer](https://help.gooddata.com/display/doc/Create+an+Insight+with+Analytical+Designer). See [AFM](50_custom__execution.md).

**aggregation** is a general term for the process by which information is gathered and expressed in summary form. Aggregation means:

* The process of computing measures using aggregation functions applied to a set of numerical facts \(such as sum, average, minimum, maximum, and so on\). See [Aggregate Table Data](https://help.gooddata.com/pages/viewpage.action?pageId=86794631#PivotTables-Tabletotals).

* The process of breaking down measures by attributes and attribute values. For example, date-related attributes that are useful in these breakdowns include Day, Month, and Year.

**attribute** is a descriptor used to break apart measures and provide context to report data. Attributes dictate how measures are calculated and represented. Attributes may contain one or more labels, which are display values for the attribute \(see **displayForm**\).

## B

**bar chart** is a chart that allows you to visually compare discrete categories of data. See [Bar Chart](afm_react_components.md#charts).

## D

**displayForm**, or **attribute label**, is a different means of representing an attribute \(see **attribute** \). For example, the `Name` attribute might have labels for `Firstname` and `Lastname`.

**drilling** is the process of 'zooming in' on a single measure value by filtering out all other data and breaking that value across a new attribute of interest.

## F

**filter** is a set of characteristics applied to a visualization or measure to remove values from computed and displayed values. For example, if you are building a regional sales chart, you may want to apply a filter to limit the display of Total Sales data to a specified region. See [Dashboard Filters](https://help.gooddata.com/display/doc/Dashboard+Filters).

## G

**gdc-catalog-export** is a GoodData UI SDK tool for exporting a list of catalog items and date datasets from a GoodData workspace. See [gdc-catalog-export](02_start__catalog_export.md).

## K

**KPI \(Key Performance Indicator\)** is a type of performance measurement that evaluate a person's or organization's success in a particular activity. See [KPI Dashboards](https://help.gooddata.com/display/doc/KPI+Dashboards).

## M

**measure** is aggregation of facts or counts of distinct attribute values, which are represented as numbers in reports. Measures represent what is being measured in a report; for example, sum of sales or average salary.

## S

**stacked bar chart** is a chart that enables the display of the composite categories that make up each bar. These charts may be useful in simplifying the display of a busy bar chart. See [Stacked Bar Chart](https://help.gooddata.com/display/doc/Stacked+Bar+Chart).

## R

**resultSpec** is an object that defines the structure of data inside an AFM \(see**AFM**\) based on your preferences. For example, you can define a custom sorting of data. See [Result Specification \(resultSpec\)](50_custom__result.md).

## V

**visualization** is a view into a specific part of your data defined by AFM, ResultSpec and a configuration object, usually in a form of a table or graph \(see **AFM** and **resultSpec**\).

**Visualization component** is a component of the GoodData UI SDK that renders a table or graph displaying data defined by a combination of AFM, ResultSpec \(see **AFM** and **resultSpec**\) and a configuration object.
