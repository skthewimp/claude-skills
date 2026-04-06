---
name: data-visualization
description: Use when creating any chart, plot, graph, or data visualization in ANY language (R, Python, JavaScript, etc.). Triggers on words like plot, chart, graph, visualize, histogram, scatter, heatmap, bar chart, time series, facet.
---

# Data Visualization Style

Tufte-inspired warm minimalist aesthetic rooted in R/ggplot2. The canonical style is ggplot2 + ggthemes::theme_tufte(). All other languages must reproduce this ggplot look - never the other way around. In Python, prefer plotnine over matplotlib.

## Colors
- Background: #e5e1d8 (warm beige) - panel AND plot, never white/grey
- Primary data: #5f3946 (dark maroon)
- Secondary/baseline: #9c9280 (grey-brown)
- Dividers: #c8c0aa (tan)
- All text: #3C3C3C (charcoal, never pure black)
- Accents: firebrick3 for highlights, blue3 for contrast
- Many categories: ColorBrewer Set1, never default color cycles

## Rules
- ALWAYS static (PNG/SVG). Never interactive (no Plotly/D3/Bokeh)
- Remove ALL gridlines. If needed, only subtle horizontal major in grey90
- Remove top/right spines. Thin remaining lines (0.2-0.3)
- Bold axis text, bold facet labels, bold title
- Subtitle in #5f3946, smaller than title
- NO legends - direct label on data. If unavoidable, bottom with no title
- Remove x-axis title when obvious
- Facets: reorder by metric (never alphabetical), free scales
- Maximize data-ink ratio. Every mark must encode data
- Annotations belong ON the chart, not in a legend

## R (primary/canonical)
```r
ggthemes::theme_tufte() +
theme(panel.grid = element_blank(),
  panel.background = element_rect(fill = '#e5e1d8', linewidth = 0),
  plot.background = element_rect(fill = '#e5e1d8', linewidth = 0),
  axis.text = element_text(face = 'bold', color = '#3C3C3C'),
  axis.line.y = element_line(colour = 'black', linewidth = 0.2),
  axis.ticks.x = element_blank(),
  legend.position = 'none')
```

## Python (plotnine preferred, matplotlib fallback)
```python
from plotnine import *

theme_karthik = (
    theme_minimal(base_size=11) +
    theme(
        panel_grid=element_blank(),
        panel_background=element_rect(fill='#e5e1d8', size=0),
        plot_background=element_rect(fill='#e5e1d8', size=0),
        axis_text=element_text(weight='bold', color='#3C3C3C'),
        axis_line_y=element_line(color='black', size=0.2),
        axis_ticks_major_x=element_blank(),
        legend_position='none',
        plot_title=element_text(weight='bold', color='#3C3C3C'),
        plot_subtitle=element_text(size=8, color='#5f3946'),
    )
)
```

matplotlib fallback (only when plotnine can't handle the chart):
```python
fig.patch.set_facecolor('#e5e1d8')
ax.set_facecolor('#e5e1d8')
ax.spines['top'].set_visible(False)
ax.spines['right'].set_visible(False)
ax.spines['left'].set_linewidth(0.3)
ax.spines['bottom'].set_linewidth(0.3)
ax.spines['left'].set_color('#3C3C3C')
ax.spines['bottom'].set_color('#3C3C3C')
ax.tick_params(colors='#3C3C3C', labelsize=9)
for label in ax.get_xticklabels() + ax.get_yticklabels():
    label.set_fontweight('bold')
ax.grid(False)
plt.savefig('output.png', dpi=150, facecolor='#e5e1d8')
```

## Preferred chart types (in order)
1. Line + point (time series)
2. Segment/range plots (bands: historical vs current)
3. Small multiples/faceted (category comparisons)
4. Horizontal bar (ranked categories)
5. Heatmap + viridis (matrices)
Avoid: pie, donut, 3D, stacked bars, radar
