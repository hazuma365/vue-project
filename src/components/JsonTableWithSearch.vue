<template>
    <div class="json-table-container">
      <h2>JSONデータテーブル</h2>
      
      <!-- 検索機能 -->
      <div class="search-container">
        <input
          v-model="searchQuery"
          type="text"
          placeholder="検索キーワードを入力..."
          class="search-input"
        />
        <select v-model="searchColumn" class="search-select">
          <option value="all">すべての列</option>
          <option v-for="column in columnList" :key="column" :value="column">{{ column }}</option>
        </select>
      </div>
  
      <!-- データローディング状態 -->
      <div v-if="loading" class="loading">データを読み込み中...</div>
      <div v-else-if="error" class="error">エラーが発生しました: {{ error }}</div>
      <div v-else>
        <!-- 検索結果のサマリー -->
        <div class="search-results">
          {{ filteredData.length }} 件表示 (全 {{ tableData.length }} 件中)
        </div>
        
        <!-- データテーブル -->
        <table v-if="filteredData.length > 0" class="data-table">
          <thead>
            <tr>
              <th 
                v-for="column in columnList" 
                :key="column" 
                @click="sortBy(column)" 
                class="sortable"
              >
                {{ column }}
                <span v-if="sortKey === column" class="sort-indicator">
                  {{ sortOrder === 'asc' ? '▲' : '▼' }}
                </span>
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in filteredData" :key="index">
              <td v-for="column in columnList" :key="column">{{ item[column] }}</td>
            </tr>
          </tbody>
        </table>
        <div v-else class="no-data">検索条件に一致するデータがありません</div>
      </div>
    </div>
  </template>
  
  <script>
  // Viteでは直接JSONファイルをインポート可能
  import jsonData from '@/assets/data.json'
  
  export default {
    name: 'JsonTableWithSearch',
    data() {
      return {
        tableData: [],
        loading: true,
        error: null,
        searchQuery: '',
        searchColumn: 'all',
        sortKey: '',
        sortOrder: 'asc',
        columnList: []
      }
    },
    computed: {
      filteredData() {
        let data = [...this.tableData];
        
        // 検索機能
        if (this.searchQuery) {
          const searchLower = this.searchQuery.toLowerCase();
          data = data.filter(item => {
            if (this.searchColumn === 'all') {
              // すべての列で検索
              return Object.values(item).some(value => 
                String(value).toLowerCase().includes(searchLower)
              );
            } else {
              // 特定の列のみ検索
              return String(item[this.searchColumn]).toLowerCase().includes(searchLower);
            }
          });
        }
        
        // ソート機能
        if (this.sortKey) {
          data.sort((a, b) => {
            const aValue = a[this.sortKey];
            const bValue = b[this.sortKey];
            
            // 数値の場合は数値比較、それ以外は文字列比較
            const compareResult = typeof aValue === 'number' && typeof bValue === 'number'
              ? aValue - bValue
              : String(aValue).localeCompare(String(bValue));
              
            return this.sortOrder === 'asc' ? compareResult : -compareResult;
          });
        }
        
        return data;
      }
    },
    created() {
      // Viteでは直接インポートしたJSONを使用
      this.initializeData();
    },
    methods: {
      initializeData() {
        try {
          // 直接インポートしたJSON（Vite方式）を使用
          this.tableData = jsonData;
          
          // 最初の要素からカラムリストを作成
          if (this.tableData.length > 0) {
            this.columnList = Object.keys(this.tableData[0]);
          }
          
          this.loading = false;
        } catch (error) {
          this.error = `データの初期化中にエラーが発生しました: ${error.message}`;
          this.loading = false;
          console.error('データの初期化エラー:', error);
        }
      },
      
      // ソート機能
      sortBy(key) {
        // 同じキーをクリックした場合はソート順を切り替え
        this.sortOrder = (this.sortKey === key && this.sortOrder === 'asc') ? 'desc' : 'asc';
        this.sortKey = key;
      }
    }
  }
  </script>
  
  <style scoped>
  .json-table-container {
    max-width: 100%;
    margin: 0 auto;
    padding: 20px;
    font-family: Arial, sans-serif;
  }
  
  .search-container {
    display: flex;
    margin-bottom: 20px;
    gap: 10px;
  }
  
  .search-input {
    flex: 1;
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    font-size: 14px;
  }
  
  .search-select {
    padding: 10px;
    border: 1px solid #ddd;
    border-radius: 4px;
    background-color: white;
    font-size: 14px;
    min-width: 150px;
  }
  
  .search-results {
    margin-bottom: 10px;
    font-size: 14px;
    color: #666;
  }
  
  .data-table {
    width: 100%;
    border-collapse: collapse;
    margin-top: 10px;
    box-shadow: 0 1px 3px rgba(0,0,0,0.1);
  }
  
  .data-table th, .data-table td {
    border: 1px solid #0a0000;
    padding: 12px 15px;
    text-align: left;
  }
  
  .data-table th {
    background-color: #0a0000;
    font-weight: bold;
  }
  
  .data-table th.sortable {
    cursor: pointer;
    position: relative;
    user-select: none;
  }
  
  .data-table th.sortable:hover {
    background-color: #0a0000;
  }
  
  .sort-indicator {
    margin-left: 5px;
    font-size: 12px;
  }
  
  .data-table tr:nth-child(even) {
    background-color: #0a0000;
  }
  
  .data-table tr:hover {
    background-color: #0a0000;
  }
  
  .loading, .error, .no-data {
    padding: 20px;
    text-align: center;
    background-color: #0a0000;
    border-radius: 4px;
    margin: 10px 0;
  }
  
  .loading {
    color: #2c3e50;
  }
  
  .error {
    color: #e74c3c;
  }
  
  .no-data {
    color: #7f8c8d;
  }
  </style>