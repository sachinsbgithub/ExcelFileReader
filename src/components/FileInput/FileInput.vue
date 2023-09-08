<template>
  <div class="file-input">
    <div
      class="drop-zone"
      @dragover.prevent
      @drop="handleDrop"
      @click="cliclToUpload">
      <h3>Drag and drop files here or click to select files</h3>
      <input
        id="file-input"
        type="file"
        ref="fileInput"
        accept=".xlsx, .xls, .csv"
        @change="onInputChange"
        multiple />
      <br />
    </div>
    <div class="file-types">
      <h3>Supproted formats .xlsx</h3>
      <h3>.xls</h3>
      <h3>.csv</h3>
    </div>
    <div class="table-container">
      <table>
        <thead>
          <tr>
            <th v-for="header in tableHeaders" :key="header">{{ header }}</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(item, index) in tableData" :key="index">
            <td v-for="header in tableHeaders" :key="header">
              {{ item[header] }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>
<script>
import "./FileInput.css";
import * as XLSX from "xlsx";
export default {
  data: () => ({
    tableHeaders: [],
    tableData: [],
    uploadedFiles: [],
  }),
  methods: {
    cliclToUpload() {
      document.getElementById("file-input").click();
    },
    handleFileInput() {
      const files = this.$refs.fileInput.files;
      this.onInputChange(files);
    },
    handleDrop(event) {
      console.log(event);
      event.preventDefault();
      const files = event.dataTransfer.files;
      this.onInputChange(files);
    },
    onInputChange(event) {
      const files = event.target.files;
      if (files.length === 0) return;
      console.log(files);
      for (let i = 0; i < files.length; i++) {
        var self = this;
        const file = files[i];
        if (
          file.type !=
          "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet"
        )
          return alert(" ⚠️ Upload  XLSX, XLS, CSV files only");
        return new Promise((resolve) => {
          const reader = new FileReader();
          reader.onload = (e) => {
            const data = e.target.result;
            const workbook = XLSX.read(data, { type: "array" });
            const firstSheetName = workbook.SheetNames[0];
            const worksheet = workbook.Sheets[firstSheetName];
            const header = self.getHeaderList(worksheet);
            const results = XLSX.utils.sheet_to_json(worksheet);
            console.log(header, results);
            self.tableHeaders = header;
            self.tableData = results;
            resolve();
          };
          reader.readAsArrayBuffer(file);
        });
      }
    },
    getHeaderList(sheet) {
      const headers = [];
      const range = XLSX.utils.decode_range(sheet["!ref"]);
      let column;
      const Row = range.s.r;
      for (column = range.s.c; column <= range.e.c; ++column) {
        const cell = sheet[XLSX.utils.encode_cell({ c: column, r: Row })];
        let header = "Column" + column;
        if (cell && cell.t) header = XLSX.utils.format_cell(cell);
        headers.push(header);
      }
      return headers;
    },
  },
};
</script>
