<template>
    <div>
      <h2>JSONデータテーブル</h2>
      <div v-if="loading">データを読み込み中...</div>
      <div v-else-if="error">エラーが発生しました: {{ error }}</div>
      <div v-else>
        <table v-if="tableData.length > 0" class="data-table">
          <thead>
            <tr>
              <th v-for="(value, key) in tableData[0]" :key="key">{{ key }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in tableData" :key="index">
              <td v-for="(value, key) in item" :key="key">{{ value }}</td>
            </tr>
          </tbody>
        </table>
        <div v-else>データがありません</div>
      </div>
    </div>
  </template>
  
  <script>
  import data from '@/assets/data.json'
  export default {
    name: 'JsonTable',
    data() {
      return {
        tableData: [],
        loading: true,
        error: null
      }
    },
    mounted() {
      this.fetchData()
    },
    methods: {
      fetchData() {
        this.tableData = data
        this.loading = false
      }
    }
  }
  </script>
  
  <style scoped>
  .data-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 20px;
  }
  
  .data-table th, .data-table td {
    border: 1px solid #020009;
    padding: 8px;
    text-align: left;
  }
  
  .data-table th {
    background-color: #0a0000;
    font-weight: bold;
  }
  
  .data-table tr:nth-child(even) {
    background-color: #070000;
  }
  
  .data-table tr:hover {
    background-color: #0d0202;
  }
  </style>