<script>
  import { MDBCol, MDBRow } from "mdbsvelte";
  import Bar from "svelte-chartjs/src/Bar.svelte";

  export let labels = [];
  export let borderData = [];

  let data = {
    labels: labels,
    datasets: [
      {
        label: "Number of Borders",
        data: borderData,
        backgroundColor: [
          "rgba(255, 134,159,0.4)",
          "rgba(98,  182, 239,0.4)",
          "rgba(255, 218, 128,0.4)",
          "rgba(113, 205, 205,0.4)",
          "rgba(170, 128, 252,0.4)",
          "rgba(255, 177, 101,0.4)",
        ],
        borderWidth: 2,
        borderColor: [
          "rgba(255, 134, 159, 1)",
          "rgba(98,  182, 239, 1)",
          "rgba(255, 218, 128, 1)",
          "rgba(113, 205, 205, 1)",
          "rgba(170, 128, 252, 1)",
          "rgba(255, 177, 101, 1)",
        ],
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
    <Bar {data} {options} />
  </MDBCol>
</MDBRow>
