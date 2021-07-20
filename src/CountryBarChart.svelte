<script>
  import { MDBCol, MDBRow } from "mdbsvelte";
  import Bar from "svelte-chartjs/src/Bar.svelte";

  export let labels = [];
  export let borderData = [];

  let colors = [];

  $: if (colors) {
    for (let i = 0; i < labels.length; i++) {
      colors.push([
        Math.floor(Math.random() * 255),
        Math.floor(Math.random() * 255),
        Math.floor(Math.random() * 255),
      ]);
    }
    colors = colors;
  }

  const plugins = [
    {
      id: "custom_canvas_background_color",
      beforeDraw: (chart) => {
        const ctx = chart.canvas.getContext("2d");
        ctx.save();
        ctx.globalCompositeOperation = "destination-over";
        ctx.fillStyle = "white";
        ctx.fillRect(0, 0, chart.width, chart.height);
        ctx.restore();
      },
    },
  ];

  $: data = {
    labels: labels,
    datasets: [
      {
        label: "Number of Borders",
        data: borderData,
        backgroundColor: colors
          ? colors.map((c) => `rgba(${c.join(",")}, 0.4)`)
          : [],
        borderWidth: 2,
        borderColor: colors ? colors.map((c) => `rgba(${c.join(",")},1)`) : [],
      },
    ],
  };

  let options = {
    responsive: true,
    scales: {
      xAxes: [
        {
          barPercentage: 1,
          gridLines: {
            display: true,
            color: "rgba(0, 0, 0, 0.1)",
          },
        },
      ],
      yAxes: [
        {
          gridLines: {
            display: true,
            color: "rgba(0, 0, 0, 0.1)",
          },
          ticks: {
            beginAtZero: true,
          },
        },
      ],
    },
  };
</script>

<MDBRow>
  <MDBCol md="8" class="mx-auto">
    <Bar {data} {options} {plugins} />
  </MDBCol>
</MDBRow>
