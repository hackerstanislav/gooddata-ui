---
title: Table
sidebar_label: Table
copyright: (C) 2007-2018 GoodData Corporation
id: version-6.0.0-table_component
original_id: table_component
---

Table shows data in columns and rows. In tables, you can choose to display only attributes (without any measures). Also, tables have higher limits for the number of datapoints to display than charts.

**NOTE:** The table component will be deprecated in the future. Use the [pivot table](10_vis__pivot_table_component.md) instead.

![Table Component](assets/table.png "Table Component")

## Structure

```jsx
import '@gooddata/react-components/styles/css/main.css';
import { Table } from '@gooddata/react-components';

<Table
    projectId={<workspace-id>}
    measures={<measures>}
    sdk={<sdk>}
    …
/>
```

## Example

```jsx
const measures = [
    {
        measure: {
            localIdentifier: 'franchiseFeesIdentifier',
            definition: {
                measureDefinition: {
                    item: {
                        identifier: franchiseFeesIdentifier
                    }
                }
            },
            format: '#,##0'
        }
    }
];

const attributes = [
    {
        visualizationAttribute: {
            displayForm: {
                identifier: monthDateIdentifier
            },
            localIdentifier: 'month'
        }
    }
];

<div style={{ height: 300 }}>
    <Table
        projectId={workspaceId}
        measures={measures}
        attributes={attributes}
    />
</div>
```

## Properties

| Name | Required? | Type | Description |
| :--- | :--- | :--- | :--- |
| projectId | true | string | The workspace ID |
| measures | false | [Measure[]](50_custom__execution.md#measure) | An array of measure definitions (either measure or attribute definition must be provided for the table to render properly) |
| attributes | false | [Attribute[]](50_custom__execution.md#attribute) | An array of attribute definitions (either measure or attribute definition must be provided for the table to render properly) |
| totals | false | [Total[]](30_tips__table_totals.md) | An array of total definitions |
| filters | false | [Filter[]](30_tips__filter_visual_components.md) | An array of filter definitions |
| config | false | [ChartConfig](15_props__chart_config.md) | The configuration object |
| sortBy | false | [SortItem[]](50_custom__result.md#sorting) | An array of sort definitions |
| locale | false | string | The localization of the table. Defaults to `en-US`. For other languages, see the [full list of available localizations](https://github.com/gooddata/gooddata-react-components/tree/master/src/translations). |
| drillableItems | false | [DrillableItem[]](15_props__drillable_item.md) | An array of points and attribute values to be drillable. |
| sdk | false | SDK | A configuration object where you can define a custom domain and other API options |
| ErrorComponent | false | Component | A component to be rendered if this component is in error state. See [ErrorComponent](15_props__error_component.md).|
| LoadingComponent | false | Component | A component to be rendered if this component is in loading state. See [LoadingComponent](15_props__loading_component.md).|
| onError | false | Function | A callback when component updates its error state |
| onLoadingChanged | false | Function | A callback when component updates its loading state |

<!-- These internals are intentionally undocumented
| afterRender | false | Function | A callback after component is rendered |
| dataSource | false | DataSource class | A class that is used to resolve AFM |
| environment | false | string | An Internal property that changes behaviour in Analytical Designer and KPI Dashboards |
| height | false | number | Height of the component in pixels |
| pushData | false | Function | A callback after AFM is resolved |
-->
