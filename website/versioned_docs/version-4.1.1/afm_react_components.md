---
title: AFM React Components
sidebar_label: AFM React Components
copyright: (C) 2007-2018 GoodData Corporation
id: version-4.1.1-afm_react_components
original_id: afm_react_components
---

The element where you are inserting a React component in must have the height and width set up. Otherwise, the visualization will not work correctly.

Though you can use either object URIs or object identifiers, we recommend that you use the **object identifiers**, which are consistent across your domain regardless of the GoodData workspace they live in. That is, an object used in any workspace within your domain would have the _same_ object identifier in_any_of those workspaces\). To get a list of catalog items and date datasets from a GoodData workspace in form of a JavaScript object, use [gdc-catalog-export](02_start__catalog_export.md).

## Charts

### Parameters

| Name | Required? | Type |
| :--- | :--- | :--- |
| afm | true | [AFM](50_custom__execution.md) |
| projectId | true | string |
| resultSpec  | false | [Result Specification \(resultSpec\)](50_custom__result.md) |
| config  | false | [ChartConfig](15_props__chart_config.md) |

### Structure

```javascript
import '@gooddata/react-components/styles/css/main.css';
import { AfmComponents } from '@gooddata/react-components';
 
const { BarChart } = AfmComponents; // replace BarChart with ColumnChart, LineChart, or PieChart whenever needed
 
<BarChart
    afm={afm>}
    projectId="<workspace-id>"
    resultSpec={resultSpec>}
    config={<chart-config>}
/>
```

### Example

This example uses data from the GoodSales demo workspace. For testing purposes, you can use this snippet as is.

```javascript
import '@gooddata/react-components/styles/css/main.css';
import { AfmComponents } from '@gooddata/react-components';
 
const { BarChart } = AfmComponents;
 
<BarChart
    afm={{
        measures: [
            {
                localIdentifier: 'CustomMeasureID',
                definition: {
                    measure: {
                        item: {
                            identifier: 'acKjadJIgZUN' // can be referenced from the exported catalog
                        }
                    }
                },
                alias: '# of Activities'
            }
        ],
        attributes: [
            {
                localIdentifier: 'a1',
                displayForm: {
                    identifier: 'label.activity.type'
                }
            }
        ]
    }}
    projectId="la84vcyhrq8jwbu4wpipw66q2sqeb923"
    resultSpec={}
/>
```

## Table

### Parameters

| Name | Required? | Type |
| :--- | :--- | :--- |
| afm | true | [AFM](50_custom__execution.md) |
| projectId | true | string |
| resultSpec  | false | [Result Specification \(resultSpec\)](50_custom__result.md) |

### Structure

```javascript
import '@gooddata/react-components/styles/css/main.css';
import { AfmComponents } from '@gooddata/react-components';
 
const { Table } = AfmComponents;
 
<Table
    afm={<afm>}
    projectId="<workspace-id>"
    resultSpec={<resultSpec>}
/>
```

### Example

This example uses data from the GoodSales demo workspace. For testing purposes, you can use this snippet as is.

```javascript
import '@gooddata/react-components/styles/css/main.css';
import { AfmComponents } from '@gooddata/react-components';
 
const { Table } = AfmComponents;
 
<Table
    afm={{
        measures: [
            {
                localIdentifier: 'CustomMeasureID',
                definition: {
                    measure: {
                        item: {
                            identifier: 'acKjadJIgZUN' // can be referenced from the exported catalog
                        }
                    }
                },
                alias: '# of Activities'
            }
        ],
        attributes: [
            {
                localIdentifier: 'a1',
                displayForm: {
                    identifier: 'label.activity.type'
                }
            }
        ]
    }}
    projectId="la84vcyhrq8jwbu4wpipw66q2sqeb923"
    resultSpec={}
/>
```
