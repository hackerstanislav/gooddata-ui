---
title: AFM
sidebar_label: AFM
copyright: (C) 2007-2018 GoodData Corporation
id: version-4.1.1-afm
original_id: afm
---

**AFM**\(Attribute - Filter - Measure\) is unified input for the [GoodData DataLayer](data_layer.md).

AFM is acombination of attributes, measures and filters that describes a query you want to execute. In terms of underlying API, it is similar to when you are creating an insight using [Analytic Designer](https://help.gooddata.com/display/doc/Create+an+Insight+with+Analytical+Designer).

## Structure

```javascript
{
    measures: [], // Optional; the default is []
    attributes: [], // Optional; the default is []
    filters: [], // Optional; the default is []
    nativeTotals: [] // Optional; the default is []
}
```

## Measure

Measures inside an AFM are represented by an array of the following objects, each of which represents a single measure:

```javascript
// Items of AFM.measures
// Type: IMeasure
{
    localIdentifier: '<measure-local-identifier>',
    // Type: SimpleMeasureDefinition
    definition: {
        measure: {
            // Type: ObjQualifier
            item: {
                identifier: '<measure-identifier>'    // Or uri: '<measure-uri>'
            },
            aggregation: 'sum', // Optional; By default 'sum'; Possible values: 'sum' | 'count' | 'avg' | 'min' | 'max' | 'median' | 'runsum'
            filters: [],        // Optional; By default []; Type: CompatibilityFilter[]
            computeRatio: true  // Optional; By default false
        }
    },
    alias: 'Custom measure title',  // Optional; Overrides default measure title
    format: '#,##0.00'  // Optional; Overrides default measure format
}

```

`item` either contains a measure URL:

```javascript
item: {
    uri: '<measure-uri>'
}
```

...or a measure identifier:

```javascript
item: {
    identifier: '<measure-identifier>'
}

```

Besides `uri` or `identifier`, a measure requires a `localIdentifier` string that uniquely identifies the measure in the context of the current AFM. This is used in dimension definitions, sorting, and any other places where you need to target a measure or an attribute.

Though you can use either object URIs or object identifiers \(`ObjQualifier = IObjUriQualifier | IObjIdentifierQualifier`\), we recommend that you use the **object identifiers**, which are consistent across your domain regardless of the GoodData workspace they live in. That is, an object used in any workspace within your domain would have the_same_object identifier in_any_of those workspaces\).

To get a list of catalog items and date datasets from a GoodData workspace in form of a JavaScript object, use [gdc-catalog-export](gdc-catolog-export.md)).

### Aggregation inside a measure

Each measure can specify `aggregation` of data. Aggregation is represented by a string value that defines the aggregation type.

| Type | Description |
| :--- | :--- |
| `'sum'` | Returns a sum of all numbers in the set |
| `'count'` | Counts unique values of a selected attribute in a given dataset determined by the second attribute parameter |
| `'avg'` | Returns the average value of all numbers in the set; null values are ignored |
| `'min'` | Returns the minimum value of all numbers in the set |
| `'max'` | Returns the maximum value of all numbers in the set |
| `'median'` | Counts the statistical median - an order statistic that gives the "middle" value of a sample. If the "middle" falls between two values, the function returns average of the two middle values. Null values are ignored. |
| `'runsum'` | Returns a sum of numbers increased by the sum from the previous value \(accumulating a sum incrementally\) |

### Filters in a measure definition

Each measure can be filtered by attribute filters. Filters are represented by an array of `FilterItem` objects. Measure attribute filters use the same `FilterItem` interface as [AFM global filters](50_custom__execution.md).

Only one filter of the [`DateFilter`](50_custom__execution.md#date-filter) type is allowed in a measure's filter definition.

When both a measure filter of the [`DateFilter`](50_custom__execution.md#date-filter) type and an AFM global filter of the [`DateFilter`](50_custom__execution.md#date-filter)type are set, the measure date filter overrides the AFM global date filter for this measure \(global date filters are still applied to other measures that do not have a measure date filter defined\).

### Show as a percentage

When an AFM is executed on the GoodData platform, the result metric data is by default returned as raw values \(numbers\).

If you want the metric data to be displayed as a percentage instead, add a `computeRatio` property and set it to `true`.

When `computeRatio` is not specified, it defaults to `false`, and values from execution are displayed as numbers.

### Period-over-period

To enable period-over-period \(PoP\), use the `PopMeasureDefinition` structure instead of `SimpleMeasureDefinition` and reference the original measure by the `measureIdentifier` property.

For examples, see PoP [with a measure defined directly from the identifier](https://confluence.intgdc.com/display/VS/AFM#AFM-Period-over-periodwithmeasuredefineddirectlyfromidentifier) and [with a measure defined by reference in AFM](https://confluence.intgdc.com/display/VS/AFM#AFM-Period-over-periodwithmeasuredefinedbyreferenceinAFM).

`PopMeasureDefinition` is represented by the following structure:

```javascript
// Type: IPopMeasureDefinition
definition: {
    popMeasure: {
        measureIdentifier: '<measure-local-identifier>',    // reference to localIdentifier in afm.measures
        // Type: IObjUriQualifier
        popAttribute: {
            uri: '<measure-uri>'    // or identifier: '<measure-identifier>'
        }
    }
}
```

## Attribute

Each attribute requires localIdentifier and `displayForm`.

* `localIdentifier` \(string\) is specified by the attribute's `displayForm` identifier.
* `type` \(string\) can be either `date` or `attribute`.

```javascript
// Type: IAttribute
{
    localIdentifier: '<attribute-local-identifier>',
    // Type: ObjQualifier
    displayForm: {
        identifier: '<attribute-displayForm-identifier>'    // Or uri: '<attribute-displayForm-uri>'
    },
    alias: 'My attribute'   // Optional; Overrides default attribute title
}
```

## Filter

```javascript
...
// Optional; By default []; Type: CompatibilityFilter[]
filters: [
    // Type: IAbsoluteDateFilter
    {
        absoluteDateFilter: {
            dataSet: {
                identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
            },
            from: '1985-04-10', // Supported string format 'YYYY-MM-DD'
            to: '2017-11-30' // Supported string format 'YYYY-MM-DD'
        }
    },
    // Type: IRelativeDateFilter
    {
        relativeDateFilter: {
            dataSet: {
                identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
            },
            granularity: 'GDC.time.date', // Supported values: 'GDC.time.date' | 'GDC.time.week' | 'GDC.time.month' | 'GDC.time.quarter' | 'GDC.time.year'
            from: -1, // Supported values: +-INT
            to: 1 // Supported values: +-INT
        }
    },
    // Type: IPositiveAttributeFilter
    {
        positiveAttributeFilter: {
            displayForm: {
                identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
            },
            in: ['<attribute-element-uri-1>', '<attribute-element-uri-2>'] // Attribute elements currently support only uri
        }
    },
    // Type: INegativeAttributeFilter
    {
        negativeAttributeFilter: {
            displayForm: {
                identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
            },
            notIn: ['<attribute-element-uri-1>', '<attribute-element-uri-2>'] // Attribute elements currently support only uri
        }
    },
    // Type: IExpressionFilter
    {
        value: '{goodsalesdemo.adf} IN ({goodsalesdemo.adf?1})'
    }
]
...
```

Both global filters and measure filters can use `in` or `notIn` and are always interpreted as an intersection of all individual filters \(`f1 AND f2 AND f3...)`.

All attributes, `popAttribute`'s and filters are defined via the `displayForm` identifier.

### DateFilter

A date filter limits data processing to the selected date intervals.

Date filters can be of the following types:

* **Absolute date filter:** you set an interval based on two exact dates
* **Relative date filter:** you set an interval that is relative to the current date \(for example, last week\)

```javascript
...
// Optional; By default []; Type: CompatibilityFilter[]
filters: [
    // Type: IAbsoluteDateFilter
    {
        absoluteDateFilter: {
            dataSet: {
                identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
            },
            from: '2017-07-31', // Supported string format 'YYYY-MM-DD'
            to: '2017-08-29' // Supported string format 'YYYY-MM-DD'
        }
    }
]
...
```

**Absolute date filter** An absolute date filter specifes a time interval within two dates: start date and end date \(in this exact order\). For example, 2017-07-31 - 2017-08-29.

**Format:**

```javascript
from: 'YYYY-MM-DD',
to: 'YYYY-MM-DD',
// e.g.
from: '2017-07-31',
to: '2017-08-29',
```

**Example:**

Interval between 2017, July 31, and 2017, August 29, inclusive:

```javascript
// Type: IAbsoluteDateFilter
{
    absoluteDateFilter: {
        dataSet: {
            identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
        },
        from: '2017-07-31', // Supported string format 'YYYY-MM-DD'
        to: '2017-08-29' // Supported string format 'YYYY-MM-DD'
    }
}
```

**Relative date filter**

A relative filter specifies a time interval that is relative to the current date. For example, last week, next month, and so on.

**Format:**

```javascript
from: number,
to: number,
// e.g.
from: -7,
to: 7,
```

* `0` for the current day, week, month, quarter, or year \(depending on the chosen granularity\)
* `-1` for the previous period
* `-` _`n`_ for the _n_th previous period

**Granularity:**

* `'GDC.time.date'` \(day granularity\)
* `'GDC.time.week'` \(week granularity\)
* `'GDC.time.month'` \(month granularity\)
* `'GDC.time.quarter'` \(quarter granularity\)
* `'GDC.time.year'` \(year granularity\)

**Example:**

Last 7 days \(yesterday and 6 days before\):

```javascript
// Type: IRelativeDateFilter
{
    relativeDateFilter: {
        dataSet: {
            identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
        },
        granularity: 'GDC.time.date',   // Supported values: 'GDC.time.date' | 'GDC.time.week' | 'GDC.time.month' | 'GDC.time.quarter' | 'GDC.time.year'
        from: -7,   // positive or negative whole numbers
        to: -1  // positive or negative whole numbers
    }
}
```

Last 12 months including the current month:

```javascript
// Type: IRelativeDateFilter
{
    relativeDateFilter: {
        dataSet: {
            identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
        },
        granularity: 'GDC.time.month',  // Supported values: 'GDC.time.date' | 'GDC.time.week' | 'GDC.time.month' | 'GDC.time.quarter' | 'GDC.time.year'
        from: -11,  // positive or negative whole numbers
        to: 0   // positive or negative whole numbers
    }
}
```

Last quarter only:

```javascript
// Type: IRelativeDateFilter
{
    relativeDateFilter: {
        dataSet: {
            identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
        },
        granularity: 'GDC.time.quarter',    // Supported values: 'GDC.time.date' | 'GDC.time.week' | 'GDC.time.month' | 'GDC.time.quarter' | 'GDC.time.year'
        from: -1,   // positive or negative whole numbers
        to: -1  // positive or negative whole numbers
    }
}
```

### AttributeFilter

Attribute filters can be of the following types:

* **Positive** **attribute filters** list only those items whose attribute elements' URIs are included in the `in` property array.
* **Negative attribute filters** lists only those items whose attribute elements' URIs are _not_ included in the  `notIn` property array.

Currently, attribute elements only support URIs, not identifiers.

**PositiveAttributeFilter**

```javascript
// Type: IPositiveAttributeFilter
{
    positiveAttributeFilter: {
        displayForm: {
            identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
        },
        in: ['<attribute-element-uri-1>', '<attribute-element-uri-2>'] // Attribute elements currently support only uri
    }
},
```

**NegativeAttributeFilter**

```javascript
// Type: IPositiveAttributeFilter
{
    negativeAttributeFilter: {
        displayForm: {
            identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
        },
        notIn: ['<attribute-element-uri-1>', '<attribute-element-uri-2>'] // Attribute elements currently support only uri
    }
},
```

## Native Total

Native totals in the AFM structure represent a definition of the data needed for computing correct results.

### Definition

```javascript
...
nativeTotals: [
    {
        measureIdentifier: string       // local measure identifier on which total is defined
        attributeIdentifiers: string[]  // subset of local attribute identifiers in AFM defining total placement
    },
    ...
]
```

### Prerequisites

Native total items must be in sync with [result specification \(ResultSpec\)](50_custom__result.md)and its dimension totals. If they are not in sync, it is treated as a bad execution request.

### Limitations

Native total are curretly supported only for:

* Table visualizations
* Grand native totals
  * `nativeTotal.attributeIdentifiers` is an empty array.

### Defining native totals

See [Table Totals in ExecutionObject](table_totals_in_execution_context.md).

### Example

```javascript
...
nativeTotals: [
    {
        measureIdentifier: '<measure-local-identifier-1>',
        attributeIdentifiers: [] // only Grand totals are currently supported so the array should be empty
    },
    ...
]
```

## Examples

### Simple measure

```javascript
{
    measures: [
        // Type: IMeasure
        {
            definition: {
                measure: {
                    item: {
                        identifier: '<measure-identifier>'    // Or uri: '<measure-uri>'
                    }
                }
            },
            localIdentifier: '<measure-local-identifier>'
        }
    ]
}
```

### Complex measure

```javascript
// Type: IAfm
{
    measures: [
        // Type: IMeasure
        {
            localIdentifier: '<measure-local-identifier>',
            // Type: MeasureDefinition
            definition: {
                measure: {
                    // Type: ObjQualifier
                    item: {
                        identifier: '<measure-identifier>'    // Or uri: '<measure-uri>'
                    },
                    aggregation: 'count',   // Optional; By default 'sum'; Possible values: 'sum' | 'count' | 'avg' | 'min' | 'max' | 'median' | 'runsum'
                    // Optional; By default []; Type: CompatibilityFilter[]
                    filters: [
                        // Type: IAbsoluteDateFilter
                        {
                            absoluteDateFilter: {
                                dataSet: {
                                    identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
                                },
                                from: '2017-07-31', // Supported string format 'YYYY-MM-DD'
                                to: '2017-08-29' // Supported string format 'YYYY-MM-DD'
                            }
                        },
                        // Type: IPositiveAttributeFilter
                        {
                            positiveAttributeFilter: {
                                displayForm: {
                                    identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
                                },
                                in: ['<attribute-element-uri-1>', '<attribute-element-uri-2>'] // Attribute elements currently support only uri
                            }
                        },
                    ],
                    computeRatio: true      // Optional; By default false
                }
            },
            alias: 'Custom measure title',  // Optional; Overrides default measure title
            format: '#,##0.00'  // Optional; Overrides default measure format
        }
    ]
}
```

### Measure with global filters 

```javascript
// Type: IAfm
{
    measures: [
        // Type: IMeasure
        {
            localIdentifier: '<measure-local-identifier>',
            // Type: MeasureDefinition
            definition: {
                measure: {
                    // Type: ObjQualifier
                    item: {
                        identifier: '<measure-identifier>'    // Or uri: '<measure-uri>'
                    },
                    aggregation: 'count',   // Optional; By default 'sum'; Possible values: 'sum' | 'count' | 'avg' | 'min' | 'max' | 'median' | 'runsum'
                    computeRatio: true      // Optional; By default false
                }
            },
            alias: 'Custom measure title',  // Optional; Overrides default measure title
            format: '#,##0.00'  // Optional; Overrides default measure format
        }
    ],
    // Optional; By default []; Type: CompatibilityFilter[]
    filters: [
        // Type: IAbsoluteDateFilter
        {
            absoluteDateFilter: {
                    dataSet: {
                    identifier: '<date-dataset-identifier>' // Or uri: '<date-dataset-uri>'
                },
                from: '2017-07-31', // Supported string format 'YYYY-MM-DD'
                    to: '2017-08-29' // Supported string format 'YYYY-MM-DD'
            }
        },
        // Type: IPositiveAttributeFilter
 
        {
            positiveAttributeFilter: {
                displayForm: {
                    identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
                },
                in: ['<attribute-element-uri-1>', '<attribute-element-uri-2>'] // Attribute elements currently support only uri
            }
        }
    ]
 
}
```

### Period-over-period with measure defined by reference in AFM 

```javasctript
{
    measures: [
        {
            localIdentifier: '<pop-measure-local-identifier>',
            definition: {
                popMeasure: {
                    measureIdentifier: 'amountMeasure',
                    popAttribute: {
                        identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
                    }
                }
            }
        },
        {
            localIdentifier: '<measure-local-identifier>',
            definition: {
                measure: {
                    item: {
                        identifier: '<measure-identifier>'    // Or uri: '<measure-uri>'
                    }
                }
            }
        }
    ],
    attributes: [
        {
            displayForm: {
                identifier: '<attribute-displayForm-identifier>' // Or uri: '<attribute-displayForm-uri>'
            },
            localIdentifier: '<attribute-local-identifier>'
        }
    ]
}
```
