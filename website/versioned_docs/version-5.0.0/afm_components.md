---
title: AFM Components
sidebar_label: AFM Components
copyright: (C) 2007-2018 GoodData Corporation
id: version-5.0.0-afm_components
original_id: afm_components
---

In their current form, the Visual Components listed in this section, do not support data sorting (for example, by attribute or measure).

If your use case require data sorting, use the following AFM components.

The AFM components use [AFM](50_custom__execution.md) property instead of specific properties such as `measures` or `viewBy` that are used in Visual Components.

> **Warning!** AFM components are legacy elements from the previous GoodData.UI version and will be eventually deprecated.

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
    afm={<afm>}
    projectId="<workspace-id>"
    resultSpec={<resultSpec>}
    config={<chart-config>}
/>
```

### Example

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
                            identifier: '<measure-identifier>' // can be referenced from the exported catalog
                        }
                    }
                },
                alias: 'My Measure'
            }
        ],
        attributes: [
            {
                localIdentifier: 'a1',
                displayForm: {
                    identifier: '<attribute-display-form-identifier>'
                }
            }
        ]
    }}
    projectId="<workspace-id>"
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
                            identifier: '<measure-identifier>' // can be referenced from the exported catalog
                        }
                    }
                },
                alias: 'My Measure'
            }
        ],
        attributes: [
            {
                localIdentifier: 'a1',
                displayForm: {
                    identifier: '<attribute-display-form-identifier>'
                }
            }
        ]
    }}
    projectId="<workspace-id>"
    resultSpec={}
/>
```
