---
layout: minimal
title: ""
---

# Data Visualisation - Plotly Charts

<br>

## Task

Consider  the strengths and limitations of interactive sunburst plots (implemented with R). Suggest an alternative representation approach you would have taken if there were no interactive facilities. 


<br>

## Sunburst charts


A sunburst chart facilitates the visualisation of hierarchical data, with the root node at the centre, and concentric rings representing the ‘child’ data for successive hierarchical levels.  The size of each segment is proportional to its value relative to the parent segment and parent segments can be selected to drill down and re- chart where that parent becomes the root, enabling one to inspect the child proportion values more clearly.   An alternative method of visualising hierarchical data would be a treemap. 


### Strengths

- The chart enables easy visualisation of hierarchical data, and overview of the relative child to parent proportions, especially when drilling down on levels
- Multilevel, complex hierarchical data is captured in a concise and visually appealing manner
- Interactive versions enable  drill down on details by selecting level of interest
- Colour can be used to highlight specific clusters or features
 

### Limitations

- Outer layers can appear cluttered and unreadable if there are multiple segments
- Whilst proportions and general overview are visualised clearly, exact numeric values are not easily apparent
- Key insights are not always highlighted or apparent as the eye sees the overall structure.



<br><br>


<a href="https://sjackson-ds25.github.io/VisualsingData/Landing%20page.html" style="display:inline-block; padding:8px 12px; background-color:#0366d6; color:white; text-decoration:none; border-radius:4px; margin-bottom:1em;">⬅️ Return to Visualising Data</a>

