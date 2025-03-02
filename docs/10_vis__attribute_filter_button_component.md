---
id: attribute_filter_button_component
title: Attribute Filter Button
sidebar_label: Attribute Filter Button
copyright: (C) 2021 GoodData Corporation
---

The **Attribute Filter Button component** is a dropdown component that lists attribute values.

![Attribute Filter Button Component](assets/attribute_filter_button_new.png "Attribute Filter Button Component")

To implement the component, choose one of the following methods:
* You pass a callback function, which receives a list of the selected values when a user clicks **Apply**.
* The component handles the change after calling itself via the ```connectToPlaceholder``` property.

    The ```onApply``` function is not needed. Use ```onApply``` only if you need a specific callback to be fired.

Optionally, you can define what attribute values should be selected in the filter by default.

> **When implementing the Attribute Filter Button component, consider the following:**:
>
> - The GoodData platform supports only filters with attribute elements defined by their URIs.
> - GoodData Cloud and GoodData.CN support filters with attribute elements defined by their `primary key` that is equal to the title of the respective element.

## Example

In the following example, attribute values are listed and the ```onApply``` callback function is triggered when a user clicks **Apply** to confirm the selection.
The `onApply` callback receives a new filter definition that you can use to filter charts.

```jsx
import React, { useState } from "react";
import { AttributeFilterButton, Model } from "@gooddata/sdk-ui-filters";
import { newNegativeAttributeFilter } from "@gooddata/sdk-model";

import "@gooddata/sdk-ui-filters/styles/css/main.css";

import * as Md from "./md/full";

const defaultFilter = newNegativeAttributeFilter(Md.EmployeeName.Default, []);

export const AttributeFilterExample = () => {
    const [filter, setFilter] = useState(defaultFilter);

    return (
        <AttributeFilterButton
            filter={filter}
            onApply={(updatedFilter) => {
                console.log("Applying updated filter:", updatedFilter);
                setFilter(updatedFilter);
            }}
        />
    );
}
```

## Attribute Filter Button component vs. Attribute Filter component

The Attribute Filter Button component is functionally similar to the [Attribute Filter component](10_vis__attribute_filter_component.md). You can use either of them. The only difference is what the filter dropdown button looks like.

![Filter Dropdown Button](assets/attribute_filter_button_new_top_visual.png "Filter Dropdown Button")

## Define the default selection of values in the filter

To define the attribute values that should be selected in the filter by default, include those attribute values in the ```filter``` property. For more details about filtering, see [Filter Visual Components](30_tips__filter_visual_components.md).

```jsx
    <AttributeFilterButton
        filter={newPositiveAttributeFilter(
            Md.EmployeeName.Default,
            ["Abbie Adams"]
        )}
        onApply={this.onApply}
    />
```

## Define a parent-child relationship between two attribute filters

To define a parent-child relationship between two attribute filters, hand over the ```parentFilters``` and ```parentFilterOverAttribute``` properties to the filter that should become a child filter dependent on the other attribute filter.

The ```parentFilterOverAttribute``` property defines the relationship between the parent filter and the child filter. You specify this attribute in the child filter via either a reference to an attribute in the parent filter or a reference to any independent attribute common for a parent filter attribute and a child filter attribute. This attribute must represent the way how the two filters are connected.

You can define the parent filter as an [AttributeFilter](30_tips__filter_visual_components.md) or a [visualization definition placeholder](30_tips__placeholders.md).

```jsx
    <div>
        <AttributeFilterButton
            filter={parentFilter}
            onApply={setParentFilter}
        />
        <AttributeFilterButton
            filter={filter}
            parentFilters={parentFilter ? [parentFilter] : []}
            parentFilterOverAttribute={idRef("attr.restaurantlocation.locationid")}
            onApply={setFilter}
        />
    </div>
```

```jsx
    <div>
        <AttributeFilterButton
            connectToPlaceholder={parentFilterPlaceholder}
        />
        <AttributeFilterButton
            connectToPlaceholder={filterPlaceholder}
            parentFilters={parentFilterPlaceholder ? [parentFilterPlaceholder] : []}
            parentFilterOverAttribute={idRef("attr.restaurantlocation.locationid")}
        />
    </div>
```

## Properties

| Name | Required? | Type | Description |
| :--- | :--- | :--- | :--- |
| onApply | false | [OnApplyCallbackType](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-ui-filters.onapplycallbacktype.html) | A callback that contains the updated filter when the selection change is confirmed by a user |
| onError | false | (error: [GoodDataSdkError](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-ui.gooddatasdkerror.html)) => void; | A callback that is triggered when the component runs into an error |
| filter | false | [IAttributeFilter](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-model.iattributefilter.html) | The attribute filter definition |
| parentFilters | false | [AttributeFiltersOrPlaceholders](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-ui.attributefiltersorplaceholders.html) | An array of parent attribute filter definitions. This feature is not yet supported by GoodData.CN and GoodData Cloud. |
| connectToPlaceholder | false | [IPlaceholder&lt;IAttributeFilter&gt;](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-ui.iplaceholder.html) | The [visualization definition placeholder](30_tips__placeholders.md) used to get and set the value of the attribute filter |
| parentFilterOverAttribute | false | [ParentFilterOverAttributeType](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-ui-filters.parentfilteroverattributetype.html) | A reference to the parent filter attribute over which the available options are reduced, or the function called for every parent filter that returns such reference for the given parent filter |
| backend | false | [IAnalyticalBackend](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-backend-spi.ianalyticalbackend.html) | The object with the configuration related to the communication with the backend and the access to analytical workspaces |
| workspace | false | string | The [workspace](02_start__execution_model.md#where-do-measures-and-attributes-come-from) ID |
| locale | false | [ILocale](https://sdk.gooddata.com/gooddata-ui-apidocs/docs/sdk-ui.ilocale.html) | The localization of the component. Defaults to `en-US`. For other languages, see the [full list of available localizations](https://github.com/gooddata/gooddata-ui-sdk/blob/master/libs/sdk-ui/src/base/localization/Locale.ts). |
| fullscreenOnMobile | false | boolean | If `true`, adjusts the filter to be properly rendered on mobile devices |
| title | false | string | A custom label to show on the dropdown button |
| titleWithSelection | false | string | The label displays the attribute title and also the applied selection. This option is not applied, if `title` property is set. |
| hiddenElements | false | string[] | Specify elements that will be exluded from the selection list. Currently, elements can be specified only by their URIs. This feature is not yet supported by GoodData.CN and GoodData Cloud. |
| staticElements | false | string[] | Provide elements to show in the selection list instead of loading them from the backend. |


**NOTE:** The ```uri``` property (the URI of the attribute displayForm used in the filter) and the ```identifier``` property (the identifier of the attribute displayForm used in the filter) are **deprecated**. Do not use them.
To define an attribute, use the ```filter``` or ```connectToPlaceholder``` property.

## Customize AttributeFilterButton components
> AttributeFilterButton component customizations are still in a beta stage.
> We appreciate any [feedback and experiences](support_options) that can help us improve this feature in the future.

If you want to customize the look of the AttributeFilterButton, you can provide your own components for rendering of its specific parts.

```jsx
    <AttributeFilterButton
        filter={newNegativeAttributeFilter(Md.EmployeeName.Default, [])}
        onApply={setFilter}
        // Provide your own component for rendering "Apply" and "Cancel" buttons
        DropdownActionsComponent={CustomActions}
        // Provide your own component for rendering of the attribute element
        ElementsSelectItemComponent={CustomItem}
    />
```

See the table below with the [customization properties](#customization-properties) to check all the customization possibilities, or see the [live example]().

### Accessing internal AttributeFilterButton context
In some cases, properties provided to the custom components may not be sufficient for you. In this case, you can use `useAttributeFilterContext` hook to obtain the full internal state of the component, and obtain the data and callbacks you need.
```jsx
    const { attribute } = useAttributeFilterContext();
```

Currently, we recommend to use the component customizations mainly for smaller tweaks of the AttributeFilter UI. In case you need a really specific custom UI that differs a lot from the AttributeFilter component, see how to implement [fully custom attribute filter](create_custom_attribute_filter).

## Customization Properties

| Name | Required? | Type | Description |
| :--- | :--- | :--- | :--- |
| ErrorComponent | false | Component | A component to be rendered if the initialization of the attribute filter fails. |
| LoadingComponent | false | Component | A component to be rendered if the initialization of the attribute filter is running. |
| DropdownButtonComponent | false | Component | A component to be rendered instead of the default dropdown button. ![Dropdown Button](assets/attribute_filter_button_dropdown_button.png "Dropdown Button") |
| DropdownBodyComponent | false | Component | A component to be rendered instead of the default dropdown body. ![Dropdown Body](assets/attribute_filter_dropdown_body.png "Dropdown Body") |
| DropdownActionsComponent | false | Component | A component to be rendered instead of the default dropdown actions. ![Dropdown Actions](assets/attribute_filter_dropdown_actions.png "Dropdown Actions")  |
| ElementsSearchBarComponent | false | Component | A component to be rendered instead of the default elements search bar. ![Elements Search Bar](assets/attribute_filter_elements_search_bar.png "Elements Search Bar") |
| ElementsSelectComponent | false | Component | A component to be rendered instead of the default elements select. ![Elements Select](assets/attribute_filter_elements_select.png "Elements Select") |
| ElementsSelectLoadingComponent | false | Component | A component to be rendered instead of the default elements select loading. ![Elements Select Loading](assets/attribute_filter_elements_select_loading.png "Elements Select Loading") |
| ElementsSelectItemComponent | false | Component | A component to be rendered instead of the default elements select item. ![Elements Select Item](assets/attribute_filter_elements_select_item.png "Elements Select Item")  |
| ElementsSelectActionsComponent | false | Component | A component to be rendered instead of the default elements select actions. ![Elements Select Actions](assets/attribute_filter_elements_select_actions.png "Elements Select Actions") |
| ElementsSelectErrorComponent | false | Component | A component to be rendered instead of the default elements select error. ![Elements Select Error](assets/attribute_filter_elements_select_error.png "Elements Select Error")  |
| EmptyResultComponent | false | Component | A component to be rendered instead of the default empty result. ![Empty Result](assets/attribute_filter_empty_result.png "Empty Result") |
| StatusBarComponent | false | Component | A component to be rendered instead of the default status bar. ![Status Bar](assets/attribute_filter_status_bar.png "Status Bar") |
