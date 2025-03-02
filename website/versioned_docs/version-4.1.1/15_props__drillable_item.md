---
title: DrillableItem
sidebar_label: DrillableItem
copyright: (C) 2007-2018 GoodData Corporation
id: version-4.1.1-drillable_item
original_id: drillable_item
---

To turn on eventing and drilling, specify at least one drillableItem. Drillable items consist of measures, attribute displayForms defined by their identifier or URI, or attribute values defined by their URI. Visualization points that intersect any defined measures, attributes, or attribute values become drillable and will emit events when interacted with.

## Structure

```javascript
drillableItems: [
    {
        identifier: <identifier>  // metric or attribute displayForm identifier
        // or
        uri: <uri>    // metric, attribute displayForm, or attribute value uri
    },
    ...
]
```

## Example

```javascript
{ identifier: 'label.owner.department' } // or { uri: '/gdc/md/workspaceHash/obj/1027' }
```
