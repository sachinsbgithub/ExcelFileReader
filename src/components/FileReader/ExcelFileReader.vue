<template>
  <div class="file-reader-container">
    <header>
      <div class="logo-container">
        <img class="logo" src="../../assets/logo.png" />
        <img class="logo" src="../../assets/Excel-logo.png" />
      </div>
      <h1>Excel File Reader Using Vue js</h1>
    </header>
    <main>
      <div clsss="file-input-comp">
        <FileInput />
      </div>
      <button
        @click="scrollToTop"
        class="scroll-to-top-button"
        v-show="showScrollButton">
        Scroll to Top
      </button>
    </main>
  </div>
</template>
<script>
import "./FileReader.css";
import FileInput from "../FileInput/FileInput.vue";
export default {
  components: { FileInput },
  data: () => ({
    showScrollButton: false,
  }),
  methods: {
    handleScroll() {
      const scrollThreshold = 200;
      this.showScrollButton = window.scrollY > scrollThreshold;
    },
    scrollToTop() {
      window.scrollTo({ top: 0, behavior: "smooth" });
    },
  },
  created() {
    window.addEventListener("scroll", this.handleScroll);
  },
  beforeDestroy() {
    window.removeEventListener("scroll", this.handleScroll);
  },
};
</script>
