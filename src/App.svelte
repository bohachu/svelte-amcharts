<script>
  import { Tabs, TabList, TabPanel, Tab } from "./tabs.js";
  import { onMount } from "svelte";

  // workaround for svelte repl
  let scripts_loaded = 0;
  const num_scripts = 3;
  function handleScriptLoaded(event) {
    console.dir(event);
    scripts_loaded++;
    if (scripts_loaded == num_scripts) {
      console.log("all scripts loaded. load chart");
      am4core.ready(amcharts4_ready_handler);
    }
  }

  function get_data_001() {
    return [
      {
        date: new Date(2018, 0, 1),
        value: 150,
        value2: 162,
        value3: 199,
      },
      {
        date: new Date(2018, 0, 2),
        value: 269,
        value2: 450,
        value3: 841,
      },
      {
        date: new Date(2018, 0, 3),
        value: 700,
        value2: 358,
        value3: 699,
      },
      {
        date: new Date(2018, 0, 4),
        value: 490,
        value2: 367,
        value3: 500,
      },
      {
        date: new Date(2018, 0, 5),
        value: 500,
        value2: 485,
        value3: 369,
      },
      {
        date: new Date(2018, 0, 6),
        value: 550,
        value2: 354,
        value3: 250,
      },
      {
        date: new Date(2018, 0, 7),
        value: 420,
        value2: 350,
        value3: 600,
      },
    ];
  }

  function get_data_002() {
    return [
      {
        date: new Date(2018, 0, 1),
        value: 999,
        value2: 888,
        value3: 777,
      },
      {
        date: new Date(2018, 0, 2),
        value: 666,
        value2: 555,
        value3: 444,
      },
      {
        date: new Date(2018, 0, 3),
        value: 700,
        value2: 358,
        value3: 699,
      },
      {
        date: new Date(2018, 0, 4),
        value: 490,
        value2: 367,
        value3: 500,
      },
      {
        date: new Date(2018, 0, 5),
        value: 500,
        value2: 485,
        value3: 369,
      },
      {
        date: new Date(2018, 0, 6),
        value: 550,
        value2: 354,
        value3: 250,
      },
      {
        date: new Date(2018, 0, 7),
        value: 420,
        value2: 350,
        value3: 600,
      },
    ];
  }

  var chart;
  function amcharts4_ready_handler() {
    /**
     * ---------------------------------------
     * This demo was created using amCharts 4.
     *
     * For more information visit:
     * https://www.amcharts.com/
     *
     * Documentation is available at:
     * https://www.amcharts.com/docs/v4/
     * ---------------------------------------
     */

    am4core.useTheme(am4themes_animated);

    // Create chart instance
    chart = am4core.create("chartdiv", am4charts.XYChart);
    chart.paddingRight = 30;

    // Add data
    chart.data = get_data_001();

    // Create axes
    var dateAxis = chart.xAxes.push(new am4charts.DateAxis());
    dateAxis.renderer.grid.template.location = 0;
    dateAxis.renderer.minGridDistance = 30;
    dateAxis.startLocation = 0.5;
    dateAxis.endLocation = 0.5;

    var valueAxis = chart.yAxes.push(new am4charts.ValueAxis());

    // Create series
    function createSeries(field, name) {
      var series = chart.series.push(new am4charts.LineSeries());
      series.dataFields.valueY = field;
      series.dataFields.dateX = "date";
      series.name = name;
      series.tooltipText = "{dateX}: [b]{valueY}[/]";
      series.strokeWidth = 2;
      series.tensionX = 0.8;
      series.stacked = true;
      series.fillOpacity = 0.2;

      return series;
    }

    var series1 = createSeries("value", "Series #1");
    var series2 = createSeries("value2", "Series #2");
    var series3 = createSeries("value3", "Series #3");

    chart.legend = new am4charts.Legend();
    chart.cursor = new am4charts.XYCursor();

    // Add scrollbar
    var scrollbar = new am4charts.XYChartScrollbar();
    scrollbar.series.push(series1);
    chart.scrollbarX = scrollbar;
  } // end amcharts4_ready_handler

  let count = 1;

  // the `$:` means 're-run whenever these values change'
  $: doubled = count * 2;
  $: quadrupled = doubled * 2;

  function handleClick() {
    chart.data=get_data_002();
    count += 1;
  }
</script>

<style type="text/css">
  main {
    width: 100%;
    height: 100%;
    display: flex;
  }
  main > * {
    flex-grow: 1;
  }
</style>

<main>
  <div id="chartdiv" />
</main>
<!-- workaround for svelte repl -->
<svelte:head>
  <script on:load={handleScriptLoaded} src="//cdn.amcharts.com/lib/4/core.js">
  </script>
  <script on:load={handleScriptLoaded} src="//cdn.amcharts.com/lib/4/charts.js">
  </script>
  <script
    on:load={handleScriptLoaded}
    src="//cdn.amcharts.com/lib/4/themes/animated.js">
  </script>
</svelte:head>

<button on:click={handleClick}> Count: {count} </button>

<p>{count} * 2 = {doubled}</p>
<p>{doubled} * 2 = {quadrupled}</p>

010
