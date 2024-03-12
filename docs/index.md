---
toc: false
theme: [light, dark, alt, wide]
---
# Repo size over time

<style>

.kpi {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-family: var(--sans-serif);
  margin: 4rem 0 8rem;
  text-wrap: balance;
  text-align: center;
}

.kpi h1 {
  margin: 2rem 0;
  max-width: none;
  font-size: 8vw;
  font-weight: 500;
  line-height: .2;
}

.kpi h2 {
  margin: 0;
  max-width: 34em;
  font-size: 14px;
  font-style: initial;
  font-weight: 500;
  line-height: 1.5;
}

.kpi h3 {
  margin: 0;
  max-width: 34em;
  font-size: 14px;
  font-style: initial;
  font-weight: 500;
  line-height: 3;
  color: var(--theme-foreground-muted);
}

@media (min-width: 640px) {
  .kpi h1 {
    font-size: 50px;
  }
}

</style>

```js
const repodata = FileAttachment("repodata.csv").csv({typed: true});
```

<div class="grid grid-cols-3" style="grid-auto-rows: 250px;">
  <div class="card kpi">
    <h1>5,084</h1>
    <h2>Total MB in latest commit</h2>
    <h3><img src="UpArrowIcon.png"/> 150 kb from previous</h3>
  </div>
  <div class="card kpi">
    <h1>13</h1>
    <h2>files changed in latest commit</h2>
    <h3><img src="UpArrowIcon.png"/> 1 file from previous</h3>

  </div>
  <div class="card kpi">
    <h1>5,084</h1>
    <h2>Total MB in latest commit</h2>
    <h3><img src="UpArrowIcon.png"/> 150 kb from previous</h3>
  </div>
</div>

```js
const metric_list = [
  {name: "size", aggregation: "sum", field: "size", subtitle: "Disk space size of your repo over each commit", label:"File size (KB)"},
  {name: "count", aggregation: "count", field: "type", subtitle: "File count of your repo over each commit", label: "File count"},
];

const metricInput = 
  Inputs.select(metric_list, {
    label: "Metric",
    format: (t) => t.name,
    value: metric_list.find((t) => t.name)
  });

const metric = Generators.input(metricInput)
```

<div class="grid grid-cols-1"">
  <div class="card">
  ${
    resize((width) => Plot.plot({
      title: "Repo size over time",
      subtitle: metric.subtitle,
      width,
      y: {grid: true, label: metric.label},
      x: {label: ""},
      color: {legend: true},
      marks: [
        Plot.ruleY([0]),
        Plot.areaY(repodata, Plot.groupX({y: metric.aggregation}, {x: "timestamp", y: metric.field, fill: "type", tip: true}))
      ]
    }))
  }
  ${metricInput}
  </div>
</div>
