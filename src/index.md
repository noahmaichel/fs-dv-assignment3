---
theme: dashboard
title: "Assignment #3"
toc: false
---

<div class="grid grid-cols-2" style="height: 100%">
  <div class="card grid-colspan-2">
    <h2>Title</h2>
    <h3>Subtitle</h3>
    ${
        Plot.plot({
        height: 300,
        width,
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
    }
  </div>
  <div class="card">
    <h2>Title</h2>
    <h3>Subtitle</h3>
    ${Plot.plot({
        width,
        height: 600,
  style: {
    fontSize: "20px"
  },
  marginLeft: 120,
  marginRight: 40,
  marginBottom: 60,
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
      textAnchor: "start"
    }),
    Plot.ruleX([0])
  ]
})}
  </div>
  <div class="card">
    <h2>Title</h2>
    <h3>Subtitle</h3>
    ${Plot.plot({
        width,
        height: 600,
  x: {
    grid: true
  },
  style: {
    fontSize: "20px"
  },
  marginLeft: 180,
  marginBottom: 60,
  color: {
    legend: true,
    domain: data.map(d => d.Customer_Segment),
    range: ["#C0C0C0", "#A0A0A0", "#4C9CD4", "#707070", "#3DA44D"]
  },
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
})}
  </div>
</div>

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
.sort((a, b) => a[0] - b[0]);

const batteryByType = Array.from(
  d3.rollup(
    data,
    v => d3.mean(v, d => +d.Battery_Capacity_kWh),
    d => d.Vehicle_Type
  ),
  ([vehicleType, meanCapacity]) => ({ vehicleType, meanCapacity })
)
.sort((a, b) => d3.descending(a.meanCapacity, b.meanCapacity))
.map((d, i) => ({ ...d, index: i === 0 }));
```