<script>
  import html2canvas from "html2canvas";
  import { jsPDF } from "jspdf";
  import CountryData from "./CountryData.svelte";

  let pdfGenerated = 0;

  //convert everything in body element to a pdf file
  function generatePdf() {
    let pdf = new jsPDF("landscape");
    html2canvas(document.querySelector("#topCountries"), {
      useCORS: true,
    }).then(function (canvas) {
      let img = new Image();
      img.src = canvas.toDataURL("image/png");
      img.onload = function () {
        pdf.addImage(
          img,
          0,
          0,
          pdf.internal.pageSize.width,
          pdf.internal.pageSize.height
        );
        pdfGenerated++;
        printPdf(pdf);
      };
    });
    html2canvas(document.querySelector("#borderingCountries"), {
      useCORS: true,
    }).then(function (canvas) {
      let img = new Image();
      img.src = canvas.toDataURL("image/png");
      img.onload = function () {
        pdf.addPage();
        pdf.addImage(
          img,
          0,
          0,
          pdf.internal.pageSize.width,
          pdf.internal.pageSize.height
        );
        pdfGenerated++;
        printPdf(pdf);
      };
    });
  }

  function printPdf(pdf) {
    if (pdfGenerated === 2) {
      pdf.save("country-data.pdf");
      pdfGenerated = 0;
    }
  }
</script>

<svelte:head>
  <title>Country Data</title>
</svelte:head>

<body class="has-background-link-light">
  <section class="hero is-small is-primary">
    <div class="hero-body">
      <p class="title">Country Data</p>
      <p class="subtitle">Jordan Aspinall</p>
      <button on:click={generatePdf}>createPdf</button>
    </div>
  </section>

  <section class="section">
    <main>
      <CountryData />
    </main>
  </section>
</body>

<style lang="scss" global>
  @import "bulma/bulma";
</style>
