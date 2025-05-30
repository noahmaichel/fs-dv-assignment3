---
title: "Report"
---

# Electrifying Insights: An Exploratory Analysis of Electric Vehicle Adoption Trends in 2023
## Report prepared by Group 4.

## Summary

EV adoption varies by region, customer segment, and product features. This report extends on the findings on the dashboard and asks:

> Which factors most influenced global EV sales in 2023?

Using 531 sales records, region, brand, model, battery, discounts, and customer data,we applied visual analytics (Observable + Plot) to explore how features, pricing, and demographics affect revenue and adoption.

Findings show high-income buyers prefer fast-charging models. Tesla leads in revenue via premium pricing, despite modest sales. Discounts had little revenue impact, suggesting brand loyalty matters more. The report provides actionable insights and a transparent, replicable analysis for technical users.

## Key Findings

- North America and Oceania lead the EV markets by revenue.
- Kia leads in revenue, showing strong brand and premium pricing power.
- High-income buyers prefer fast-charging EVs, highlighting the appeal of convenience and advanced features.
- Mid-sized batteries are most popular, offering a balance of cost and range.

## The Problem

Electric vehicles (EVs) are key to reducing transportation emissions and are driving major investments in technology, incentives, and infrastructure. However, uneven sales across regions, segments, and vehicle types raise questions about what truly drives adoption.

Broad trends are known, but data-driven links between features, behavior, and performance are lacking. This project fills that gap by analyzing 531 EV sales records, including brand, region, battery type, discounts, and demographics, to identify key adoption drivers and inform smarter decision-making for policymakers and industry experts.

## Visual Analysis and Results

### Temporal Trends in EV Registrations

```js
Plot.plot({
  x: {
    type: "time",
    label: "Month",
    ticks: d3.utcMonth.every(3),
    tickFormat: d3.utcFormat("%b %Y"),
    grid: true
  },
  y: {
    label: "Number of Registrations",
    domain: [0, 80],
    grid: true
  },
  marks: [
    Plot.line(registrationsByMonth, {
      x: d => d[0],
      y: d => d[1],
      stroke: "steelblue",
      strokeWidth: 3,
      tip: true
    }),
    Plot.dot(registrationsByMonth, {
      x: d => d[0],
      y: d => d[1],
      fill: "steelblue",
      r: 4
    }),
    Plot.ruleY([0])
  ]
})
```

_Same chart as on the dashboard._

Monthly registration data reveals seasonal patterns in EV adoption. Peaks in January and June of 2023 suggest biannual cycles, likely tied to model launches or incentive timing. Understanding these trends can help explain consumer behavior and manufacturer strategies.

### Geographic Distribution of EV Registrations

```js
Plot.plot({
  x: {
    label: "Region",
    padding: 0.3
  },
  y: {
    label: "Number of Registrations",
    domain: [0, 170],
    grid: true
  },
  color: {
    type: "categorical",
    range: ["#4e79a7", "#f28e2b", "#76b7b2", "#e15759", "#9c755f", "#59a14f"],
  },
  marks: [
    Plot.barY(registrationsByRegion, {
      x: d => d[0],
      y: d => d[1],
      fill: d => d[0],
      title: d => `${d[0]}: ${d[1]}`
    }),
    Plot.text(registrationsByRegion, {
      x: d => d[0],
      y: d => d[1] / 2,
      text: d => d[1],
      fill: "white",
      fontSize: 10
    }),
    Plot.ruleY([0])
  ]
})
```

Regional EV adoption varies widely. <span style="font-weight: 600; color: #e15759;">North America</span> (154) and <span style="font-weight: 600; color: #9c755f;">Oceania</span> (136) have the most registrations, while <span style="font-weight: 600; color: #76b7b2;">Europe</span> (31) and <span style="font-weight: 600; color: #59a14f">South America</span> (36) have the fewest. These disparities are likely driven by differences in policy, infrastructure, and consumer awareness. More detailed data is needed to confirm long-term trends.

### Tracking the Front-Runners in EV Launches

```js
Plot.plot({
  x: { axis: null },
  y: { axis: null },
  color: {
    legend: true,
    type: "ordinal",
    domain: uniqueManufacturers,
    range: brandColors
  },
  marks: [
    Plot.barX(registrationsByManufacturerStacked, {
      x: d => d.count,
      y: () => "Market Share",
      fill: d => d.manufacturer,
      title: d => `${d.manufacturer}: ${d.percentage}%`
    }),
    Plot.text(registrationsByManufacturerStacked, {
      x: d => d.mid,
      y: () => "Market Share",
      text: d => `${d.percentage}%`,
      fill: "white",
      textAnchor: "middle",
      fontSize: 10
    })
  ],
  height: 60,
  caption: `Share of new registrations based on ${d3.sum(registrationsByManufacturerStacked, d => d.count).toLocaleString()} total registrations`
})
```

A brand-level analysis shows that Asian manufacturers are leading EV model launches, accounting for 59.4% of new registrations. <span style="font-weight: 600; color: #a6cee3;">Kia</span> is in the lead with 15.3%, followed by <span style="font-weight: 600; color: #e17c05">BMW</span>, <span style="font-weight: 600; color: #e7298a;">Hyundai</span>, and <span style="font-weight: 600; color: #1b9e77">Ford</span>. <span style="font-weight: 600; color: #b2df8a">Toyota</span> and <span style="font-weight: 600; color: #fdbf6f">Tesla</span> have smaller shares, suggesting a more selective approach to rollouts. These findings underscore Asia’s growing influence and innovation in the global EV market.

### Regional Revenue Distribution Reveals Strong Markets in North America and Oceania

```js
Plot.plot({
  x: { label: "Regions" },
  y: { grid: true, label: "Revenue in million USD" },
  color: {
    type: "categorical",
    range: ["#4e79a7", "#f28e2b", "#76b7b2", "#e15759", "#9c755f", "#59a14f"],
  },
  caption: "Aggregated revenue per region in 2023.",
  marks: [
    Plot.barY(revenueByRegion, {
        x: "0",
        y: d => d[1] / 1_000_000,
        fill: d => d[0],   
        tip: true
    }),
    Plot.text(revenueByRegion, {
      x: d => d[0],
      y: d => d[1] / 1_000_000 / 2,
      text: d => d3.format(",.1~f")(d[1] / 1_000_100),
      fill: "white",
      fontSize: 10
    }),
    Plot.ruleY([0])
  ]
})
```

<span style="font-weight: 600; color: #e15759;">North America</span> ($725 million) and <span style="font-weight: 600; color: #9c755f;">Oceania</span> ($703 million) lead in EV revenue, confirming their strong market positions. Meanwhile, <span style="font-weight: 600; color: #76b7b2">Europe</span> and <span style="font-weight: 600; color: #59a14f">South America</span> lag behind, though Africa's $559 million suggests growing demand, possibly driven by premium models or incentives. These patterns reveal key regional differences in EV adoption and spending.

### Market Share by Brand based on revenue

```js
Plot.plot({
  x: { axis: null },
  y: { axis: null },
  color: {
    legend: true,
    type: "ordinal",
    domain: uniqueManufacturers,
    range: brandColors
  },
  marks: [
    Plot.barX(revenueByManufacturerStacked, {
      x: d => d.count,
      y: () => "Market Share",
      fill: d => d.manufacturer,
      title: d => `${d.manufacturer}: ${d3.format(",")(d.count)} USD (${d.percentage}%)`
    }),
    Plot.text(revenueByManufacturerStacked, {
      x: d => d.mid,
      y: () => "Market Share",
      text: d => `${d.percentage}%`,
      fill: "white",
      textAnchor: "middle",
      fontSize: 10
    })
  ],
  height: 60,
  caption: `Based on total revenue per brand`
})
```

A revenue-based analysis reveals a competitive EV market with no dominant brand. <span style="font-weight: 600; color: #a6cee3">Kia</span> is in the lead with 14.6% market share, followed by <span style="font-weight: 600; color: #e17c05">BMW</span> with 13.7% and <span style="font-weight: 600; color: #e7298a">Hyundai</span> with 13.5%. Together, Asian brands hold over 60% of the market, showing strong sector influence. <span style="font-weight: 600; color: #fdbf6f">Tesla</span> holds a modest 6% share, suggesting a niche or regional role. The data reflects diverse pricing strategies and widespread market power.

### Trucks Offer the Largest Mean Capacity

```js
Plot.plot({
  marginLeft: 74,
  marginRight: 24,
  x: { label: "Battery Capacity in kWh" },
  y: {
    label: "Vehicle Type",
    domain: batteryByType.map(d => d.vehicleType)
  },
  color: {
    legend: true,
    domain: batteryByType.map(d => d.vehicleType),
    range: ["#FF7F0E", "#2A2A2A", "#505050", "#888888", "#B0B0B0", "#E0E0E0"],
    label: "Highest Battery Capacity"
  },
  marks: [
    Plot.barX(batteryByType, {
      x: "meanCapacity",
      y: "vehicleType",
      fill: ["#FF7F0E", "#2A2A2A", "#505050", "#888888", "#B0B0B0", "#E0E0E0"],
      tip: true,
      title: d =>
        `${d.vehicleType}
Mean Capacity: ${d.meanCapacity.toFixed(1)} kWh`
    }),
    Plot.text(batteryByType, {
      x: "meanCapacity",
      y: "vehicleType",
      text: d => d3.format(".1~f")(d.meanCapacity),
      dx: 4,
      textAnchor: "start",
      fontSize: 10
    }),
    Plot.ruleX([0])
  ]
})
```

_Same chart as on the dashboard._

Battery capacity varies by vehicle type, reflecting design priorities. <span style="font-weight: 600; color: #FF7F0E">Trucks</span> lead with an average of 71.5 kWh, followed by coupes and SUVs with an average of 70 kWh. Crossovers average 67.2 kWh, suggesting a focus on efficiency and affordability. Larger vehicles require more energy, while smaller ones strike a balance between range and practicality.

### Regional Differences Shape EV Customer Profiles

```js
Plot.plot({
  x: {
    grid: true
  },
  color: {
    legend: true,
    domain: data.map(d => d.Customer_Segment),
    range: ["#C0C0C0", "#A0A0A0", "#4C9CD4", "#707070", "#3DA44D"]
  },
  marginLeft: 96,
  marks: [
    Plot.barX(
      data,
      Plot.groupY(
        { x: "count" },
        { y: "Region", fill: "Customer_Segment", tip: true }
      )
    ),
    Plot.ruleX([0])
  ]
})
```

_Same chart as on the dashboard._

EV buyer profiles vary by region. In Asia, there is a balance of <span style="font-weight: 600; color: #4C9CD4">budget-conscious</span> and <span style="font-weight: 600; color: #3DA44D">eco-conscious</span> buyers, while <span style="font-weight: 600; color: #4C9CD4">budget-conscious</span> buyers dominate in North America and Oceania. High- and middle-income buyers are most prevalent in Africa, North America, and Oceania. These patterns reflect regional values and economics, guiding tailored growth strategies.

### Correlation Between Battery Capacity and Vehicle Price Varies by Charging Capability

```js
Plot.plot({
  grid: true,
  caption: "Excluding outliers in the visual representation.",
  x: {
    label: "Battery Capacity per Unit (kWh)",
    domain: [0.1, 1.2]
  },
  y: {
    label: "Price per Unit (€)",
    domain: [0, 40000],
    tickFormat: d3.format(",")
  },
  symbol: {
    legend: true,
    range: ["square2", "cross"],
  },
  color: {
    range: ["steelblue", "tomato"],
  },
  marks: [
    Plot.dot(combinedFastChargingData, {x: "capacityPerUnit", y: "revenuePerUnit", symbol: "group", fill: "group"}),
    Plot.linearRegressionY(combinedFastChargingData.filter(d => d.group != "Fast Charging"), {
      x: d => d.capacityPerUnit,
      y: d => d.revenuePerUnit,
      stroke: "tomato",
    }),
    Plot.dot(combinedFastChargingData, {x: "capacityPerUnit", y: "revenuePerUnit", symbol: "group", fill: "group"}),
    Plot.linearRegressionY(combinedFastChargingData.filter(d => d.group == "Fast Charging"), {
      x: d => d.capacityPerUnit,
      y: d => d.revenuePerUnit,
      stroke: "steelblue",
    }),
  ]
})
```

Battery size affects EV prices, but the extent of the effect depends on <span style="font-weight: 600; color: steelblue">fast-charging</span>.

<span style="font-weight: 600; color: tomato">Without fast-charging</span>, larger batteries increase the price, demonstrating added value.

However, <span style="font-weight: 600; color: steelblue">fast-charging</span>, the price impact flattens, suggesting that consumers view charging speed as the primary premium feature.
This suggests two key strategies: bundle both features for premium models or skip fast charging in budget EVs to stay competitive.

## What Does This All Mean?

EV data shows global trends: registrations spike in January and June, likely due to new models or seasonal buying. North America, and Oceania lead in registrations and revenue; the data shows strong engagement from eco-conscious and budget buyers. Trucks have bigger batteries, but fast-charging capability, not battery size, drives price shaping the future of EVs. No single brand dominates, but Kia, Hyundai, and Nissan lead sales.

For automakers, timing model launches in January and June makes sense. Oceania, Africa, and Asia are key growth markets. Governments should focus on infrastructure, especially in Africa and South America.

## Take-Home Message

The EV market is growing fast, but it plays out differently across regions. Adoption patterns vary by region, season, and consumer priorities. Asian brands are leading the charge, fast-charging tech matters more than just battery size, and opportunities are emerging in regions like Africa and Oceania. Understanding these trends helps manufacturers, policymakers, and investors make smarter, more targeted decisions. Ultimately, the key to accelerating EV adoption lies in timing, local context, and putting user needs, like charging speed and affordability, at the center of innovation.

<!-- DATA PROCESSING BELOW -->

```js
const brandColors = ["#e17c05", "#6a3d9a", "#1b9e77", "#e7298a", "#a6cee3", "#fb9a99", "#fdbf6f", "#b2df8a", "#cab2d6"];
const manufacturerNames = registrationsByManufacturerStacked.map(d => d.manufacturer);
const uniqueManufacturers = [...new Set(manufacturerNames)];
```

```js
const data = FileAttachment("data/data.csv").csv({typed: true})
```

```js
const registrationsByMonth = d3.rollups(
  data,
  v => v.length,
  d => {
    const date = new Date(d.Date);
    return date; // new Date(date.getFullYear(), date.getMonth());
  }
).map(([date, count]) => [new Date(date), count])
.sort((a, b) => a[0] - b[0])
```

```js
const registrationsByRegion = d3.rollups(
  data,
  v => v.length,
  d => d.Region
)
```

```js
const registrationsByManufacturer = d3.rollups(
  data,
  v => v.length,
  d => d.Brand
).sort((a, b) => d3.ascending(a[0], b[0]))
```

```js
const registrationsByManufacturerStacked = (() => {
  const total = d3.sum(registrationsByManufacturer, d => d[1]);
  let cumulative = 0;
  
  return registrationsByManufacturer
    .sort((a, b) => d3.ascending(a[0], b[0]))
    .map(([manufacturer, count]) => {
      const x0 = cumulative;
      cumulative += count;
      return {
        manufacturer,
        count,
        mid: x0 + count / 2,
        percentage: ((count / total) * 100).toFixed(1)
      };
    });
})()
```

```js
const revenueByManufacturer = d3.rollups(
  data,
  v => d3.sum(v, d => +d.Revenue),
  d => d.Brand
).sort((a, b) => d3.ascending(a[0], b[0]))
```

```js
const totalMarketRevenue = d3.sum(data, d => +d.Revenue)
```

```js
const revenueByManufacturerStacked = (() => {
  const total = d3.sum(revenueByManufacturer , d => d[1]);
  let cumulative = 0;
  
  return revenueByManufacturer 
    .sort((a, b) => d3.ascending(a[0], b[0]))
    .map(([manufacturer, count]) => {
      const x0 = cumulative;
      cumulative += count;
      return {
        manufacturer,
        count,
        mid: x0 + count / 2,
        percentage: ((count / total) * 100).toFixed(1)
      };
    });
})()
```

```js
const revenueByRegion = d3.rollups(
  data,
  v => d3.sum(v, d => +d.Revenue),
  d => d.Region
).sort((a, b) => d3.ascending(a[0], b[0]))
```

```js
const revenueByRegionStacked = (() => {
  const total = d3.sum(revenueByRegion , d => d[1]);
  let cumulative = 0;
  
  return revenueByRegion 
    .sort((a, b) => d3.ascending(a[0], b[0]))
    .map(([manufacturer, count]) => {
      const x0 = cumulative;
      cumulative += count;
      return {
        manufacturer,
        count,
        mid: x0 + count / 2,
        percentage: ((count / total) * 100).toFixed(1)
      };
    });
})()
```

```js
const perUnitData = Array.from(
  d3.rollup(
    data.filter(d =>
      d.Units_Sold > 0 &&
      d.Revenue > 0 &&
      d.Brand &&
      d.Model
    ),
    v => {
      const totalRevenue = d3.sum(v, d => +d.Revenue);
      const totalCapacity = d3.sum(v, d => +d.Battery_Capacity_kWh)
      const totalUnits = d3.sum(v, d => +d.Units_Sold);
      return {
        model: `${v[0].Brand} ${v[0].Model}`.trim(),
        totalRevenue,
        totalCapacity,
        totalUnits,
        revenuePerUnit: totalRevenue / totalUnits,
        capacityPerUnit: totalCapacity / totalUnits
      };
    },
    d => `${d.Brand} ${d.Model}`.trim()
  ).values()
).sort((a, b) => b.revenuePerUnit - a.revenuePerUnit)
```

```js
const perUnitDataWithFastCharging = Array.from(
  d3.rollup(
    data.filter(d =>
      d.Units_Sold > 0 &&
      d.Revenue > 0 &&
      d.Brand &&
      d.Model &&
      d.Fast_Charging_Option == "Yes"
    ),
    v => {
      const totalRevenue = d3.sum(v, d => +d.Revenue);
      const totalCapacity = d3.sum(v, d => +d.Battery_Capacity_kWh)
      const totalUnits = d3.sum(v, d => +d.Units_Sold);
      return {
        model: `${v[0].Brand} ${v[0].Model}`.trim(),
        totalRevenue,
        totalCapacity,
        totalUnits,
        revenuePerUnit: totalRevenue / totalUnits,
        capacityPerUnit: totalCapacity / totalUnits
      };
    },
    d => `${d.Brand} ${d.Model}`.trim()
  ).values()
).sort((a, b) => b.revenuePerUnit - a.revenuePerUnit)
```

```js
const perUnitDataWithoutFastCharging = Array.from(
  d3.rollup(
    data.filter(d =>
      d.Units_Sold > 0 &&
      d.Revenue > 0 &&
      d.Brand &&
      d.Model &&
      d.Fast_Charging_Option != "Yes"
    ),
    v => {
      const totalRevenue = d3.sum(v, d => +d.Revenue);
      const totalCapacity = d3.sum(v, d => +d.Battery_Capacity_kWh)
      const totalUnits = d3.sum(v, d => +d.Units_Sold);
      return {
        model: `${v[0].Brand} ${v[0].Model}`.trim(),
        totalRevenue,
        totalCapacity,
        totalUnits,
        revenuePerUnit: totalRevenue / totalUnits,
        capacityPerUnit: totalCapacity / totalUnits
      };
    },
    d => `${d.Brand} ${d.Model}`.trim()
  ).values()
).sort((a, b) => b.revenuePerUnit - a.revenuePerUnit)
```

```js
const combinedFastChargingData = [
  ...perUnitDataWithFastCharging.map(d => ({ ...d, group: "Fast Charging" })),
  ...perUnitDataWithoutFastCharging.map(d => ({ ...d, group: "No Fast Charging" }))
]
```

```js
const batteryByType = Array.from(
  d3.rollup(
    data,
    v => d3.mean(v, d => +d.Battery_Capacity_kWh),
    d => d.Vehicle_Type
  ),
  ([vehicleType, meanCapacity]) => ({ vehicleType, meanCapacity })
)
.sort((a, b) => d3.descending(a.meanCapacity, b.meanCapacity))
.map((d, i) => ({ ...d, index: i === 0 }))
```
