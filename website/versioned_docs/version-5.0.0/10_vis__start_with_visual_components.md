---
title: Start with Visual Components
sidebar_label: Start with Visual Components
copyright: (C) 2007-2018 GoodData Corporation
id: version-5.0.0-start_with_visual_components
original_id: start_with_visual_components
---

GoodData.UI comes with ready-made visual components listed in the Visual Components section. You can use these visual components as-is or customize them. You can also use the unique Visualization component that simply renders any chart that you create on the GoodData platform.

This article provides components examples and basic instructions on component usage.

## Responsive UI

Visual components are responsive by nature and take the whole space of their wrapper element. This behavior implicates that if you want to create a visualization with a specific `height` and `width`, you must specify those dimensions in the wrapper element. Otherwise, the visualization may not be visible.

### Example

```javascript
<div style={{ height: 400, width: 600 }}>
    <Visualization ... />
</div>
```

## Visual component props

The component props can be of the following types:

* **Style props** that define the style and interaction
* **Data props** that define the data returned by execution of the GoodData platform itself

The style props define style and interaction of a visualization. For more information, see the articles in the Properties section.

The data props pass measures and attributes. These props are similar to the drag and drop sections in [Analytical Designer](https://help.gooddata.com/display/doc/Analytical+Designer) and use similar names such as "View by", "Stack by" and so on.
A data prop can be a single value or an array of either the `IMeasure` or `IVisualizationAttribute` type, which is passed to the component as an object literal.

You can find more information about data props in the articles about individual components in the Visual Components section.

### Example
```js
<div style={{ height: 300 }}>
    <AreaChart
        projectId="myworkspace"
        measures={[
                      {
                          measure: {
                              localIdentifier: 'm1',
                              definition: {
                                  measureDefinition: {
                                      item: { identifier: 'aagAVA3ffiz' }
                                  }
                              },
                              format: '#,##0'
                          }
                      }
                  ]}
        viewBy={{
                    visualizationAttribute: {
                        displayForm: { uri: '/gdc/md/myworkspace/obj/851' },
                        localIdentifier: 'month'
                    }
                }}
    />
</div>
```

### How to work with attributes and measures

A measure can be referred to by its `identifier` or `uri`.

An attribute can be referred to by this attribute's identifier or by the identifier of one of the attribute's display forms (labels).
When using an attribute in a data prop, specify the identifier of the attribute's display form. However, when you are using attributes in filters, you must refer to the attribute itself, not its display form.

To find the identifier or URI of a measure or attribute, use either of the following options:

* Use [gdc-catalog-export](02_start__catalog_export.md): Download a list of attributes and measures from your workspace. In the downloaded list, find the measures and attributes that you need.
* Use [Analytical Designer](https://secure.gooddata.com/analyze).
  1. Create a visualization that uses measures and attributes that you need.
  2. Use your browser's Developer Tools and open the [Network tab](https://developers.google.com/web/tools/chrome-devtools/network-performance/reference#filter).
  3. Find requests to `/executeAfm`.
  4. Search for the [AFM](50_custom__execution.md) in the request body. It contains the required identifiers of the measures, atributes, and attribute display forms.

#### Object URI vs. object identifier
Although you can use either object URIs or object identifiers with all visual components, we recommend that you use the **object identifiers**, which are consistent across your domain regardless of the GoodData workspace they live in. That is, an object used in any workspace within your domain would have the **same** object identifier in **any** of those workspaces.

#### Type definition
You can find the TypeScript typings [here](https://github.com/gooddata/gooddata-typings/blob/v2.0.0/src/VisualizationObject.ts#L86-L102).

```ts
IMeasure = {
  measure: {
    localIdentifier: string
          // An arbitrary identifier, which is later used in sorting and filtering
    definition: IMeasureDefinition | IPoPMeasureDefinition
          // Specifies simple measure or period-over-period measure, see below
    alias: string // Optional. Alternative measure name to be displayed
    format: string // Optional. Rules for number formating, if empty measure default is used
  }
}

IMeasureDefinition = {
    measureDefinition: {
        item: { uri / identifier: string }
            // uri or identifier of specific measure from your workspace
        aggregation: string
            // Optional. Operation on the measure - one of sum, count, avg, min, max, median, runsum
        filters: VisualizationObjectFilter[]
            // Optional. Array of attribute or date filters (more at page Filter Visual Components)
        computeRatio: boolean
            // Optional. Return value as ratios from whole, useful for showing percents.
    }
}

// references another measure, but shows its values shifted back by one year (aka "period over period")
IPoPMeasureDefinition = {
    popMeasureDefinition: {
        measureIdentifier: Identifier // localIdentifier of the referenced measure
        popAttribute: { uri / identifier: string }
            // uri or identifier of attribute which is used for slicing (not the displayForm)
    }
}
```

```ts
IVisualizationAttribute = {
    visualizationAttribute: {
        localIdentifier: Identifier // An arbitrary identifier, which is later used in sorting and filtering
        displayForm: { uri / identifier: string } // The attribute`s display form
        alias: string // Optional. Alternative attribute name to be displayed
    }
}
```

## Visualization lifecycle

Visualization lifecycle is a series events that take place between mounting and rendering a visualization. During this time, a new datasource is created based on the saved visualization identifier or URI. Once its execution is resolved, the result is rendered.

Ad hoc insights, rendered using components like pie chart or table, follow the same lifecycle.

The following component props can be used as lifecycle callbacks:

| Property | Description | Parameters |
| :--- | :--- | :--- |
| onLoadingChanged | A function that is called when a loading state changes | ```{ isLoading: boolean }``` |
| onError | A function that is called when an error state changes | ```{ status: string, ...}``` |
| onLegendReady  | A function that is called when a chart legend is rendered | ```{ legendItems: [...] }``` |

### Visualization Rendered Successfully

If a visualization is successfully rendered, the following events occur:

1. The visualization is mounted and rendered immediately using a ```LoadingComponent```.
2. onLoadingChanged callback is called with a parameter ```{ isLoading: true }```.
3. onError callback is called with a parameter ```{ status: "OK" }```.
4. onLoadingChanged callback is called with a parameter ```{ isLoading: false }```.
5. The visualization is rendered.
6. onLegendReady callback is called with a parameter ```{ legendItems: [...] }```.

### Visualization Failed to Render

If an error is encountered during rendering a visualization (for example, too much data to display), the following events occur:

1. The visualization is mounted and rendered immediately using a ```LoadingComponent```.
2. onLoadingChanged callback is called with a parameter ```{ isLoading: true }```.
3. onError callback is called with a parameter ```{ status: "OK" }```.
4. onLoadingChanged callback is called with a parameter ```{ isLoading: false }```.
5. onError callback is called with the following parameter:
```json
{
    "status": "DATA_TOO_LARGE_TO_DISPLAY",
    "options": {
        "dateOptionsDisabled": false
    }
}
```
6. The visualization is rerendered using an ```ErrorComponent```.

### Flow Chart

![Visualization lifecycle chart](assets/visualization_lifecycle.png "Visualization lifecycle chart")
<!-- https://drive.google.com/open?id=1sNjUcs9s0SOn68lIvVtIE3-edw6EMiY_ -->
