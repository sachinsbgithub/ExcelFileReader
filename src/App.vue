<template>
  <div id="app">
    <ExelFileReader />
  </div>
</template>

<script>
import ExelFileReader from "@/components/FileReader/ExcelFileReader.vue";
export default {
  name: "App",
  components: { ExelFileReader },
};
</script>

<style>
body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;

  height: 100vh;
}
</style>
