---
title: Column Chart
sidebar_label: Column Chart
copyright: (C) 2007-2018 GoodData Corporation
id: version-5.0.0-column_chart_component
original_id: column_chart_component
---

Column chart shows data in vertical columns. Column charts can display one or multiple measures side by side, divided by either attribute values or by a single measure stacked by attribute values.

![Column Chart Component](assets/column_chart.png "Column Chart Component")

## Structure

```jsx
import '@gooddata/react-components/styles/css/main.css';
import { ColumnChart } from '@gooddata/react-components';

<ColumnChart
    projectId={<workspace-id>}
    measures={<measures>}
    config={<chart-config>}
    …
/>
```

## Example

```jsx
const totalSales = {
    measure: {
        localIdentifier: 'totalSales',
        definition: {
            measureDefinition: {
                item: {
                    identifier: totalSalesIdentifier
                },
                aggregation: 'sum'
            }
        },
        alias: '$ Total Sales',
        format: '#,##0'
    }
};

const month = {
    visualizationAttribute: {
        displayForm: {
            identifier: monthDateIdentifier
        },
        localIdentifier: 'month'
    }
};

<div style={{ height: 300 }}>
    <ColumnChart
        projectId={workspaceId}
        measures={[totalSales]}
        viewBy={month}
    />
</div>
```

## Properties

| Name | Required? | Type | Description |
| :--- | :--- | :--- | :--- |
| projectId | true | string | The workspace ID |
| measures | true | [Measure[]](50_custom__execution.md#measure) | An array of measure definitions |
| viewBy | false | [Attribute[]](50_custom__execution.md#attribute) | An array of attribute definitions |
| stackBy | false | [Attribute[]](50_custom__execution.md#attribute) | An array of attribute definitions |
| filters | false | [Filter[]](30_tips__filter_visual_components.md) | An array of filter definitions |
| config | false | {} | The chart configuration object |
| locale | false | string | The location string for translations |
| drillableItems | false | DrillableItem[] | An array of points and attribute values to be drillable. See [DrillableItems](15_props__drillable_item.md).|
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
