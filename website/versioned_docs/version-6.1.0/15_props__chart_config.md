---
title: Chart Config
sidebar_label: Chart Config
copyright: (C) 2007-2018 GoodData Corporation
id: version-6.1.0-chart_config
original_id: chart_config
---

This article describes the options for configuring a chart.

## Structure

```javascript
{
    colors: ['rgb(195, 49, 73)', 'rgb(168, 194, 86)'], // array of strings
    xaxis: {
        visible: true, // boolean
        labelsEnabled: true, // boolean
        rotation: 'auto', // 'auto' or numeral string
        min: '10', // numeral string
        max: '20' // numeral string
    },
    yaxis: {
        visible: true, // boolean
        labelsEnabled: true, // boolean
        rotation: 'auto', // string
        min: '30', // numeral string
        max: '40' // numeral string
    },
    secondary_yaxis: {
        visible: true, // boolean
        labelsEnabled: true, // boolean
        rotation: 'auto', // string
        min: '300', // numeral string
        max: '400', // numeral string
        measures: ['metricLocalIdentifier']
    },
    legend: {
        enabled: true, // boolean
        position: 'bottom', // 'top' | 'left' | 'right' | 'bottom'
    },
    dataLabels: {
        visible: 'auto' // 'auto' | true | false
    },
    grid: {
        enabled: true // boolean
    },
    separators: {
        thousand: ',',
        decimal: '.'
    }
}
```

## Configure colors

The following are examples of a color array:

```javascript
['rgb(195, 49, 73)', 'rgb(168, 194, 86)']

```

```javascript
['#fa0510', '#AA2030']

```

If there are fewer colors than data points, then the colors are repeated. For example, here is how colors will be used for two colors and three data points:

```javascript
['rgb(195, 49, 73)', 'rgb(168, 194, 86)', 'rgb(195, 49, 73)']
```

To change colors in a chart, provide a `config` for each component where you want to change colors, or create a wrapped components with a `config` baked in.

**NOTE:** Heatmaps use only the first color from the provided colors as the base color, and generate the other colors themselves.

```javascript
import { Visualization } from '@gooddata/react-components';

// Example of embedding a visualization with custom colors and palette options
<Visualization
    projectId=<workspace-id>
    identifier=<visualization-id>
    config={{
        colors: ['rgb(195, 49, 73)', 'rgb(168, 194, 86)']
    }}
/>
```

### Custom color palette

If you [uploaded a custom color palette](https://help.gooddata.com/pages/viewpage.action?pageId=86794489) to your workspace, the visualizations created based on the [Visualization component](10_vis__insight_view.md) use this palette instead of the default colors.

To override the uploaded custom color palette for a specific visualization, define the `colorPalette` property for this visualization. The `colorPalette` property overrides the uploaded custom palette and the `colors` property, if it has been set for this visualization.

```javascript
import { Visualization } from '@gooddata/react-components';

// Example of embedding a visualization with custom palette
<Visualization
   projectId=<workspace-id>
   identifier=<visualization-id>
   config={{
       colorPalette: [{
            guid: '01',
            fill: {
                r: 195,
                g: 49,
                b: 73
            }
        }, {
            guid: '02',
            fill: {
                r: 168,
                g: 194,
                b: 86
            }
        }, {
            guid: '03',
            fill: {
                r: 243,
                g: 217,
                b: 177
            }
        }]
    }}
/>
```

## Change legend visibility and position

* To hide the legend, set the `config.legend.enabled` property to `false`.
* To change the legend position, adjust the `config.legend.position` property \(`'left'`/`'right'`/`'top'`/`'bottom'`\).

```javascript
import { Visualization } from '@gooddata/react-components';

// Example of embedding a visualization with a custom legend position
<Visualization
    projectId=<workspace-id>
    identifier=<visualization-id>
    config={{
        legend: {
            enabled: true,
            position: 'bottom' // 'left', 'right', 'top'
        }
    }}
/>
```

## Change a separator in the number format

* To change the thousands separator, adjust the `config.separators.thousand` property.
* To change the decimal separator, adjust the `config.separators.decimal` property.

```javascript
import { Visualization } from '@gooddata/react-components';

// Example of embedding a visualization with a custom separator in the number format
<Visualization
    projectId=<workspace-id>
    identifier=<visualization-id>
    config={{
        separators: {
            thousand: ',',
            decimal: '.'
        }
    }}
/>
```

## Configure axes

* To change axis visibility, set the `config.xaxis.visible` property.
* To hide axis labels, set the `config.xaxis.labelsEnabled` property to `false`.

    **NOTE:** When `config.xaxis.visible` is set to `false`, axis labels are hidden automatically regardless of what `config.xaxis.labelsEnabled` is set to.
* To rotate axis labels, set `config.xaxis.rotation` to a desired value.
* To set the axis scale, set `config.xaxis.min` and `config.xaxis.max`.
* To show measures on a secondary axis, set `config.secondary_xaxis.measures`. If `config.secondary_xaxis.measures` is not configured, all measures are displayed on the main axis by default.

> For the properties of the Y axis, replace `xaxis` with `yaxis`.

```javascript
import { Visualization } from '@gooddata/react-components';

// Example of embedding a visualization with settings for the x-axis
<Visualization
    projectId=<workspace-id>
    identifier=<visualization-id>
    config={{
        xaxis: {
            visible: false,
            labelsEnabled: false,
            rotation: '-90',
            min: '150',
            max: '440'
        },
        secondary_xaxis: {
            visible: true,
            labelsEnabled: true,
            rotation: '-90',
            min: '1500',
            max: '4400',
            measures: ['metricLocalIdentifier1', 'metricLocalIdentifier2']
        }
    }}
/>
```

## Configure canvases

To configure data labels, set the `config.dataLabels` property.

```javascript
import { Visualization } from '@gooddata/react-components';

// Example of embedding a visualization with settings for the canvas
<Visualization
    projectId=<workspace-id>
    identifier=<visualization-id>
    config={{
        dataLabels: true,
        grid: {
            enabled: false
        }
    }}
/>
```

## Customize tooltips and fonts

To customize tooltips and fonts, [implement a custom visualization](data_layer.md).
