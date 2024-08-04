<template>
  <div>
    <table style="width:100%; margin: 100px; margin-top: 20px;">
      <thead>
        <tr>
          <th>Assest Name</th>
          <th>Deparment</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="item in data" :key="item.Asset_Name">
          <td>{{ item.Asset_Name }}</td>
          <td>{{ item.Department }}</td>
        </tr>
      </tbody>
    </table>
    <div class="button-container">
      <button @click="exportDataToCsv">Export CSV</button>
    </div>
  </div>
</template>
<script>
export default{

data() {
  return {
    data:[
        { Asset_Name: "Printer", Department: "HR"},
        { Asset_Name: "Monitor", Department: "IT"},
        { Asset_Name: "Barcode Scanner", Department: "ACCOUNT"}
    ]
  };
},

  methods: {
    exportDataToCsv(){
      const csvContent = this.convertToCSV(this.data);
      const blob = new Blob([csvContent], {type: 'text/csv;charset=utf-8'})
      const url = URL.createObjectURL(blob);
      const link = document.createElement('a');
      link.href = url;
      link.setAttribute('download', 'export_data.csv');
      link.click();
    },
    convertToCSV(data){
      const headers = Object.keys(data[0]);
      const rows = data.map(obj => headers.map(header => obj[header]));
      const headerRow = headers.join(',');
      const csvRows = [headerRow, ...rows.map(row => row.join(','))];
      return csvRows.join('\n');
    }
  }
}
</script>

<style>
table {
  margin-top: 20px;
  width: 100%;
  margin: auto;
  border-collapse: collapse;
}

th, td {
  border: 1px solid black;
  padding: 8px;
  text-align: left;
}

.button-container {
  display: flex;
  justify-content: center;
  margin-top: 20px;
}

button {
  padding: 10px 20px;
  font-size: 16px;
  cursor: pointer;
}
</style>