<template>
  <div id="app">
    <main >
      <div class="pagination">
        <Pagination 
        v-if="dataCount"
        :totalRecords="dataCount"
        v-model="page"
        @input="(n) => refreshTable(n)"
        />
        <div>

        </div>
      </div>
      <div class="tables">
        <Table 
          v-if="tableData"
          :theData="tableData"
          :config="config"
          :style="{height: '600px'}"
          @pilotsClicked="(pilots) => refreshPilots(pilots)"
        />
        <Pilots 
          v-if="pilotData"
          :theData="pilotData"
          :config="configPilots"
          :style="{height: '600px'}"
        />
      </div>
      
      
    </main>
  </div>
</template>

<script>
import Table from './components/Table.vue'
import Pagination from './components/Pagination.vue'
import Pilots from './components/Pilots.vue'

import axios from 'axios'

export default {
  
  components: {
    Table,
    Pagination,
    Pilots
  },
  data: () => ({
      tableData: undefined,
      dataCount: undefined,
      dataShown: undefined,
      pilotData: undefined,
      page: 1,
      config: [
        {
          key: 'name',
          title: 'Name',
          type: 'text'
        },
        {
          key: 'model',
          title: 'Model',
          type: 'text'
        },
        {
          key: 'manufacturer',
          title: 'Manufacturer',
          type: 'text'
        },
        {
          key: 'starship_class',
          title: 'Starship class',
          type: 'text'
        },
        {
          key: 'pilots',
          title: 'Pilots',
          type: 'array'
        }
      ],
      configPilots: [
        {
          key: 'name',
          title: 'Name',
          type: 'text'
        },
        {
          key: 'height',
          title: 'Height',
          type: 'text'
        },
        {
          key: 'mass',
          title: 'Mass',
          type: 'text'
        },
        {
          key: 'gender',
          title: 'Gender',
          type: 'text'
        }
      ]
  }),
  
  computed: {
    computedTableData () {
      if (!this.tableData) return []
      else {
        return this.tableData
      }
    }
  },

  mounted () {
    axios.get("https://swapi.dev/api/starships")
    .then(({data}) => {
      this.tableData = data.results
      // this.dataShown = data.results        // код який їсть усю оперативку
      // this.tableData = []
      // this.tableData.push(data.results)
      // let nextAvaliable = data.next;
      this.dataCount = data.count
      // while (nextAvaliable !== null){
      //   axios.get(nextAvaliable)
      //   .then(({data}) => {
      //     console.log(data)
      //     this.tableData.push(data.results)
      //     nextAvaliable = data.next;
      //   })
      // }
    })
  },
  methods: {
    refreshTable(pageNum){
      console.log(pageNum)
      // this.dataShown = this.tableData[pageNum - 1]   // вставка данних які ми вже витягнули з апі у таблицю пілотів
      axios.get(`https://swapi.dev/api/starships/?page=${pageNum}`)
      .then(({data}) => {
        this.page = pageNum
        console.log(data)
        this.tableData = data.results
      })
    },
    
    refreshPilots(pilots){  // апішці іноді буває дуже погано, тому якщо воно загрузило не всіх пілотів то почекайте
      this.pilotData = []
      pilots.forEach(element => {
        axios.get(element)
        .then(data => {
          this.pilotData.push(data.data)
        })
      });
    }
  }
}
</script>

<style>

.pagination {
  display: grid;
  grid-template-columns: 2.5fr 1fr;
}

.tables {
  display: grid;
  grid-template-columns: 2.5fr 1fr;
  column-gap: 20px;
}

body {
  font-family: Helvetica, sans-serif;
  font-weight: 400;
  margin: 0;
}

main {
  display: grid;
  justify-content: space-between; 
}

nav {
  height: 60px;
  background: white;
  font-size: 32px;
  color: white;
  display: flex;
  align-items: center;
  padding-left: 20px;
}
</style>
