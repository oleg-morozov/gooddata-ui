---
title: Embed a Visualization
sidebar_label: Embed a Visualization
copyright: (C) 2007-2018 GoodData Corporation
id: version-5.0.0-ht_embed_visualization
original_id: ht_embed_visualization
---

To embed an existing visualization created in Analytical Designer, use the `Visualization` component.

**Steps:**

1. Obtain the identifier of the visualization via [gdc-catalog-export](02_start__catalog_export.md).

2. Import the `Visualization` component from the `@gooddata/react-components` package into your app:

```javascript
import{ Visualization } from'@gooddata/react-components';
```

3. Create a `Visualization` component in your app, and provide it with the workspace ID and the visualization identifier that you obtained at Step 1:

```javascript
// This is an example of embedding a visualization from the GoodSales demo workspace.
<Visualization
    projectId="la84vcyhrq8jwbu4wpipw66q2sqeb923"
    identifier="aby3polcaFxy"
    config={{
        colors: ['rgb(195, 49, 73)', 'rgb(168, 194, 86)'],
        legend: {
            enabled: true,
            position: 'bottom'
        }
    }}
/>
```
