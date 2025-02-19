<template>
  <div>
    <input type="file" @change="handleFileUpload" />
    <div id="flipbook">
      <div v-for="(page, index) in pages" :key="index">
        <img :src="page" alt="Page" />
      </div>
    </div>
    <button @click="prevPage">Previous</button>
    <button @click="nextPage">Next</button>
  </div>
</template>

<script>
import $ from "jquery";
import "turn.js";
import { getDocument, GlobalWorkerOptions } from "pdfjs-dist";

export default {
  name: "FlipBook",
  data() {
    return {
      pages: [],
    };
  },
  methods: {
    handleFileUpload(event) {
      const file = event.target.files[0];
      if (file) {
        const fileReader = new FileReader();
        fileReader.readAsDataURL(file);
        fileReader.onload = (e) => {
          this.loadPDF(e.target.result);
        };
      }
    },

    async loadPDF(pdfUrl) {
      try {
        GlobalWorkerOptions.workerSrc =
          "https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.16.105/pdf.worker.min.js";

        const pdf = await getDocument(pdfUrl).promise;
        const totalPages = pdf.numPages;

        this.pages = [];

        for (let i = 1; i <= totalPages; i++) {
          const page = await pdf.getPage(i);
          const canvas = document.createElement("canvas");
          const context = canvas.getContext("2d");
          const viewport = page.getViewport({ scale: 2 });

          canvas.width = viewport.width;
          canvas.height = viewport.height;

          await page.render({ canvasContext: context, viewport }).promise;
          this.pages.push(canvas.toDataURL("image/png"));
        }

        // Wait until pages are loaded, then initialize Flipbook
        this.$nextTick(() => {
          this.initFlipbook();
        });
      } catch (err) {
        console.error("Error loading PDF:", err);
      }
    },

    initFlipbook() {
      this.$nextTick(() => {
        if (window.jQuery) {
          $("#flipbook").turn({
            width: 800,
            height: 500,
            autoCenter: true,
            display: "double",
          });
        } else {
          console.error("jQuery is not loaded properly.");
        }
      });
    },

    nextPage() {
      $("#flipbook").turn("next");
    },

    prevPage() {
      $("#flipbook").turn("previous");
    },
  },
};
</script>

<style>
#flipbook {
  width: 800px;
  height: 500px;
  margin: auto;
}
img {
  width: 100%;
  height: 100%;
}
</style>
