---
title: Area Chart
sidebar_label: Area Chart
copyright: (C) 2007-2018 GoodData Corporation
id: version-6.0.0-area_chart_component
original_id: area_chart_component
---

Area chart shows data as an area under a line intersecting dots. It can display either multiple measures as different areas or a single measure split by one attribute into multiple areas with points intersecting attribute values.

Areas stack by default. Alternatively, areas can overlap if config is set to ```{ stacking: false }```.

![Area Chart Component](assets/area_chart.png "Area Chart Component")

## Structure

```jsx
import '@gooddata/react-components/styles/css/main.css';
import { AreaChart } from '@gooddata/react-components';

<AreaChart
    projectId={<workspace-id>}
    measures={<measures>}
    config={<chart-config>}
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

const attribute = {
    visualizationAttribute: {
        displayForm: {
            identifier: monthDateIdentifier
        },
        localIdentifier: 'month'
    }
};

<div style={{ height: 300 }}>
    <AreaChart
        projectId={workspaceId}
        measures={measures}
        viewBy={attribute}
    />
</div>
```

## Properties

| Name | Required? | Type | Description |
| :--- | :--- | :--- | :--- |
| projectId | true | string | The workspace ID |
| measures | true | [Measure[]](50_custom__execution.md#measure) | An array of measure definitions |
| viewBy | false | [Attribute](50_custom__execution.md#attribute) | Attribute definition |
| stackBy | false | [Attribute](50_custom__execution.md#attribute) | Attribute definition |
| filters | false | [Filter[]](30_tips__filter_visual_components.md) | An array of filter definitions |
| sortBy | false | [SortItem[]](50_custom__result.md#sorting) | An array of sort definitions |
| config | false | [ChartConfig](15_props__chart_config.md) | The chart configuration object |
| locale | false | string | The localization of the chart. Defaults to `en-US`. For other languages, see the [full list of available localizations](https://github.com/gooddata/gooddata-react-components/tree/master/src/translations). |
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

The following example shows the supported `config` structure with sample values. To see descriptions of individual options, see [ChartConfig section](15_props__chart_config.md).
```javascript
{
    colors: ['rgb(195, 49, 73)', 'rgb(168, 194, 86)'],
    xaxis: {
        visible: true,
        labelsEnabled: true,
        rotation: 'auto'
    },
    yaxis: {
        visible: true,
        labelsEnabled: true,
        rotation: 'auto',
        min: '20',
        max: '30'
    },
    legend: {
        enabled: true,
        position: 'top',
    },
    dataLabels: {
        visible: 'auto'
    },
    grid: {
        enabled: true
    }
    separators: {
        thousand: ',',
        decimal: '.'
    }
}
```
