---
title: "Report"
---

# Electrifying Insights: An Exploratory Analysis of Electric Vehicle Adoption Trends in 2023
## Report prepared by Group 4.

## Summary

Electric vehicles (EVs) are precipitating a profound transformation in the automotive landscape. However, the adoption patterns of EVs exhibit significant regional, consumer segment, and product feature-related variations. In this report, we explore the following question:

> Which factors exert the most significant influence on the performance of EV sales across global markets in 2023?

Utilizing a structured dataset comprising 531 EV sales records, encompassing details such as region, brand, model, vehicle type, battery capacity, discount rates, and customer segments, we employ visual analytics techniques in Observable utilizing the Plot library. The exploratory approach that has been adopted in this study emphasizes data clarity, pattern discovery, and transparent preprocessing to understand how technical features (e.g., fast charging), pricing strategies (e.g., discounts), and customer demographics interact to drive revenue and adoption.

Our analysis indicates that high-income customers are the predominant purchasers of EVs, particularly those equipped with rapid charging capabilities. Notably, Tesla, a prominent player in the EV market, has achieved substantial revenue despite a relatively modest sales growth, suggesting a strategic pricing premium that sets it apart from competitors. A notable finding is the absence of a robust correlation between discounts and increased revenue. This observation suggests that brand loyalty and other non-price factors may play a more significant role in consumer behavior. This report presents data-driven insights pertinent to manufacturers, policymakers, and analysts seeking to comprehend or influence the EV transition. Readers will benefit from an in-depth yet clearly explained walkthrough of the analytical process, designed for replication and critique by technical audiences.

## Key Findings

Despite selling fewer units, Tesla has achieved a leading position in revenue, indicating the effectiveness of its brand equity and premium pricing strategy.

A study of consumer purchasing patterns reveals that high-income customer segments constitute the primary demographic for electric vehicle (EV) purchases, particularly for models equipped with fast-charging capabilities. This observation suggests that factors such as convenience and advanced features hold greater appeal for affluent buyers. The prevalence of fast-charging options among vehicles is a significant factor contributing to their enhanced sales performance across diverse geographical markets.

The absence of a robust correlation between discount percentages and higher revenue indicates that the implementation of aggressive price cuts may not constitute the most efficacious sales strategy within the EV market. Mid-sized battery capacities (60-80 kWh) are the most commonly sold, reflecting a balance between affordability and practical driving range.

While North America and Europe represent the most substantial markets, underperforming regions such as Oceania and South America may offer significant growth prospects if equipped with adequate infrastructure and appropriate incentives. These insights can assist manufacturers in refining their marketing and pricing strategies, and guide policymakers in their efforts to increase EV adoption through targeted incentives and infrastructure investment.

## The Problem

The following section will address the context and motivation. The global initiative to reduce carbon emissions has elevated electric vehicles (EVs) to a prominent position as a pivotal solution to mitigate greenhouse gas emissions from the transportation sector. In response, governments and industry leaders have allocated substantial resources to the development and promotion of EV technologies, financial incentives, and infrastructure. This strategic investment aims to catalyze the widespread adoption of EVs, thereby addressing both environmental and economic challenges. Despite this momentum, EV sales remain uneven across various geographical areas, customer segments, and vehicle types, prompting critical inquiries regarding the factors that truly influence adoption and the manner in which industry participants can respond.

While earlier research has examined macro-level trends in EV adoption, there is a paucity of detailed, data-driven analyses that link product characteristics, consumer behavior, and financial performance. The present project aims to address this gap by conducting a comprehensive investigation into: Which factors exert the greatest influence on EV sales and revenue outcomes across global markets? The objective of this study is to analyze a dataset of 531 sales entries, including variables such as brand, region, battery capacity, discounts, and buyer demographics, in order to identify patterns that explain which strategies and features lead to successful EV adoption. The objective of this study is to furnish insights that are pertinent to policymakers, business analysts, and data scientists who aspire to enhance their comprehension of the evolving EV landscape and facilitate more informed decision-making in a rapidly electrifying global context.

## Visual Analysis and Results

### Temporal Trends in EV Registrations

Understanding the dynamics of electric vehicle (EV) adoption requires more than just looking at total registration numbers - it demands attention to when consumers choose to adopt new models. Analyzing monthly registration trends offers insight into market behavior, seasonal influences, and potential manufacturer strategies. By examining how registrations fluctuate over time, we can uncover hidden patterns that inform both policy decisions and business strategy in the EV sector.

<div style="border: 3px solid red; padding: 20px; font-size: 2rem; font-weight: 800">
    Should we add the actual chart here again? -> Check with assignment
</div>

The line on the primary dashboard-page illustrates a clear seasonal pattern in EV registrations throughout 2023, with notable peaks in January and June, suggesting a semiannual rhythm in model launches or purchase behavior. These trends may reflect strategic release cycles by manufacturers or consumer responses to financial incentives tied to calendar cycles.

### Geographic Distribution of EV Registrations

To understand how electric vehicle (EV) adoption varies globally, it's important to examine where registrations are occurring. Geographic analysis can reveal how regional factors - such as policy incentives, infrastructure readiness, and economic conditions - shape the pace and scale of EV uptake. By analyzing the total number of registrations across continents, we gain insights into the relative maturity and momentum of EV markets worldwide.

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
  marks: [
    Plot.barY(registrationsByRegion, {
      x: d => d[0],
      y: d => d[1],
      fill: "#1f77b4",  
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

The bar chart shows that North America (154) and Oceania (136) lead in total EV registrations, while Europe (31) and South America (36) report the lowest counts in the dataset. This uneven distribution suggests that regional disparities in EV adoption remain significant, potentially driven by differences in market readiness, government support, or consumer awareness. While the data points to stronger engagement in some regions, more granular and longitudinal data would be needed to confirm long-term trends.

### Tracking the Front-Runners in EV Launches

To understand competitive dynamics in the electric vehicle (EV) market, it’s essential to look at how actively different manufacturers are expanding their model portfolios. Analyzing the distribution of new vehicle registrations by brand reveals which companies are most engaged in launching new models and how different regions or strategies might be influencing their activity. This view can shed light on market momentum and brand-level innovation in the rapidly evolving EV landscape.

```js
Plot.plot({
  x: { axis: null },
  y: { axis: null },
  color: { legend: true },
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

The data shows that all major brands are actively releasing new EV models, but Asian manufacturers in particular stand out - accounting for an accumulated 59.4%. Kia leads with a 15.3% share of new registrations, followed by BMW (13.6%), Hyundai (12.8%), and Ford (12.2%). Notably, Toyota and Tesla have smaller shares in this context, suggesting a more selective or phased rollout strategy. Overall, this underscores the strength and diversity of offerings from Asian brands, reflecting their aggressive push into the EV market and their growing global influence in shaping its future direction.

### Regional Revenue Distribution Reveals Strong Markets in North America and Oceania

To fully grasp the economic impact of electric vehicle (EV) adoption, it’s important to examine how revenue from vehicle sales is distributed across regions. This perspective helps highlight where the market generates the most financial value and indicates regions with stronger consumer purchasing power or higher-priced offerings. Understanding regional revenue distribution complements registration data by revealing where the market is not only growing in volume but also in economic significance.

```js
Plot.plot({
  x: { label: "Regions" },
  y: { grid: true, label: "Revenue in million USD" },
  caption: "Aggregated revenue per region in 2023.",
  marks: [
    Plot.barY(revenueByRegion, { x: "0", y: d => d[1] / 1_000_000, fill: "#1f77b4", tip: true }),
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

The chart shows North America ($725M) and Oceania ($703M) as the strongest markets by revenue, confirming their leadership in the global EV landscape. Europe and South America generate the lowest revenues, mirroring earlier observations from registration data. Notably, Africa’s relatively high revenue ($559M) points to emerging demand, possibly driven by premium vehicles or targeted incentives. Overall, these results reinforce the picture of North America and Oceania as mature, high-value EV markets while highlighting important regional disparities in adoption and spending.

### Market Share by Brand based on revenue

Analyzing market share through revenue provides a nuanced perspective on the competitive landscape in the electric vehicle (EV) industry. Unlike registration counts alone, revenue data reflects pricing strategies, model mix, and consumer preferences, offering a clearer picture of brand performance and market influence. This approach helps identify which manufacturers are driving financial value and how market power is distributed across players.

```js
Plot.plot({
  x: { axis: null },
  y: { axis: null },
  color: { legend: true },
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

The data reveals a relatively low market concentration, with revenue spread fairly evenly among multiple brands. Kia leads with 14.6%, followed closely by BMW (13.7%) and Hyundai (13.5%). Asian manufacturers collectively - Kia, Hyundai, Nissan, Toyota, and BYD - account for over 60% of total revenue, underscoring their dominance in the sector. Meanwhile, established German brands like BMW and Volkswagen maintain steady shares, reflecting enduring consumer trust. Notably, Tesla holds a modest 6.0% share, indicating a more niche or regionally constrained role within this dataset. Overall, the findings portray a highly competitive market landscape without a single brand commanding overwhelming dominance.

### Trucks Offer the Largest Mean Capacity

Battery capacity is a critical performance metric in electric vehicles, directly influencing driving range, charging frequency, and consumer appeal. By analyzing average battery capacity across different vehicle types, we can better understand how design priorities vary - whether favoring power, range, or efficiency. This comparison sheds light on how manufacturers tailor battery specs to meet the functional demands of each segment, from light crossovers to heavy-duty trucks.

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

The chart highlights a clear distinction in battery capacity across vehicle types. Trucks lead with an average battery size of 71.5 kWh, closely followed by coupes and SUVs, both averaging just over 70 kWh. In contrast, crossovers sit at the lower end of the spectrum with an average capacity of 67.2 kWh.

This pattern suggests that larger or utility-oriented vehicles - like trucks - are typically equipped with higher-capacity batteries, likely to support greater energy demands related to size, payload, or performance. The variation across vehicle categories may reflect differing design priorities: while trucks emphasize range and power, crossovers may favor affordability, efficiency, or urban practicality.

### Regional Differences Shape EV Customer Profiles

Understanding who purchases electric vehicles (EVs) in different regions provides valuable context for tailoring marketing strategies, product offerings, and policy measures. Customer profiles - spanning income levels and buyer motivations such as budget-consciousness or environmental concerns - reflect the diverse priorities and economic realities of EV consumers worldwide. Analyzing these regional differences helps identify where certain segments dominate and where opportunities for growth or targeted engagement exist.

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

The chart reveals notable regional variations in EV customer profiles. In Asia, Budget-Conscious and Eco-Conscious buyers are equally prevalent, illustrating a balance between affordability concerns and environmental awareness. Outside Asia, Budget-Conscious buyers dominate, particularly in North America and Oceania, where they comprise the largest share of customers. High and Middle Income buyers are most prominent in Africa, North America, and Oceania, highlighting stronger purchasing power or preference for premium EV models in these regions. The data underscores how economic factors and regional values shape EV adoption patterns, suggesting that manufacturers and policymakers should adapt their approaches accordingly.

### Correlation Between Battery Capacity and Vehicle Price Varies by Charging Capability

Battery capacity is a core technical characteristic of electric vehicles (EVs), closely linked to range, performance, and ultimately, price. Yet, this relationship is not uniform - its strength depends on whether the vehicle supports fast charging. Fast charging not only improves user experience by reducing downtime but also enhances the value proposition of larger batteries. This section explores how price sensitivity shifts when fast-charging capability is (or isn’t) present, offering insights into consumer valuation of these combined features.

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

The scatterplot reveals two distinct trends in how battery capacity influences vehicle price, segmented by the presence of fast-charging capability:

- For vehicles *without* fast charging (red crosses), there is a noticeable positive correlation between battery capacity and unit price. As battery size increases, prices also rise—suggesting that consumers do place value on extended range when it is the only available premium feature. This trend implies that in the absence of fast charging, larger batteries are treated as standalone upgrades, contributing directly to perceived value.

- For vehicles *with* fast charging (blue squares), the regression line remains relatively flat, indicating that increased battery capacity does not significantly raise vehicle prices. This suggests that consumers may see fast charging as the key value driver, and once it's present, additional battery capacity delivers diminishing returns in perceived value - or is expected as standard in higher-end models.

Strategically, this implies that premium vehicles should integrate both fast charging and higher-capacity batteries to justify their positioning, while budget-friendly models could focus on moderate battery sizes and omit fast charging to remain cost-competitive. The results highlight the importance of feature bundling and price differentiation based on charging infrastructure and performance expectations.

## What Does This All Mean?

By looking closely at the electric vehicle (EV) data, we can spot some clear trends in how electric vehicles are being adopted around the world. Registrations tend to spike twice a year, especially in January and June, which hints at a regular rhythm, maybe tied to new model launches or seasonal buying habits. Regionally, North America, Oceania, and Africa are leading the way in both the number of registrations and overall revenue. Meanwhile, Asia stands out for strong consumer engagement, particularly among eco-conscious and budget-focused buyers. When it comes to vehicle types, trucks actually have the largest batteries, but crossovers remain the most common on the road. And when we look at the market share, no one brand has taken over completely, but Asian companies like Kia, Hyundai, and Nissan are clearly out in front in terms of both sales and revenue.

These insights are valuable for a lot of different people. For carmakers, those predictable peaks in January and June could be the perfect time to roll out new models. The high demand in places like Oceania and Africa, and the steady interest in Asia, suggest that these regions are ripe for growth, with the right products and strategies.

Governments and city planners might also take note. The differences in adoption between regions show where infrastructure and incentives are working—and where they’re still needed. In places like Africa or South America, more support through charging networks or subsidies could help speed things along.

For investors and EV tech developers, there’s a key takeaway in how battery capacity relates to pricing. It turns out that fast-charging capability is a bigger driver of price than battery size alone. That means consumers may care more about how quickly their car charges than how big the battery is something that could shape the next wave of EV design and investment.

## Take-Home Message

The electric vehicle market is growing fast, but it plays out differently across regions. Adoption patterns vary by region, season, and consumer priorities. Asian brands are leading the charge, fast-charging tech matters more than just battery size, and opportunities are emerging in regions like Africa and Oceania. Understanding these trends helps manufacturers, policymakers, and investors make smarter, more targeted decisions. Ultimately, the key to accelerating electric vehicle adoption lies in timing, local context, and putting user needs, like charging speed and affordability, at the center of innovation.

<!-- DATA PROCESSING BELOW -->

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
