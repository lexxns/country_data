<script>
  import html2canvas from "html2canvas";
  import { jsPDF } from "jspdf";
  import CountryData from "./CountryData.svelte";

  let pdfGenerated = 0;

  function generatePage(pdf, querySelector, addPage) {
    html2canvas(document.querySelector(`#${querySelector}`), {
      useCORS: true,
      allowTaint: true,
      scrollX: -window.scrollX,
      scrollY: -window.scrollY,
      windowWidth: document.documentElement.offsetWidth,
      windowHeight: document.documentElement.offsetHeight,
    }).then(function (canvas) {
      let img = new Image();
      img.src = canvas.toDataURL("image/png");
      img.onload = function () {
        if (addPage) {
          pdf.addPage();
        }
        pdf.addImage(
          img,
          0,
          0,
          pdf.internal.pageSize.width,
          pdf.internal.pageSize.height
        );
        pdfGenerated++;
        showPdf(pdf);
      };
    });
  }

  function generatePdf() {
    let pdf = new jsPDF("landscape");
    generatePage(pdf, "topCountries", false);
    generatePage(pdf, "borderingCountries", true);
  }

  function showPdf(pdf) {
    if (pdfGenerated === 2) {
      window.open(pdf.output("bloburl"), "_blank");
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
