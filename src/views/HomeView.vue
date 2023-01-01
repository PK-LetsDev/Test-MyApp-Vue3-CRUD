<template>
  <div class="q-pa-md">
    <div class="q-py-md">
      <q-btn icon="add" @click="onCreate" />
    </div>
    <q-table title="FOODSALE" :rows="filteredData" :columns="columns" row-key="name">
      <template v-slot:top>
        <div style="width: 100%" class="row">
          <div class="col-9">
          </div>
          <div class="col-3">
            <q-input dense debounce="400" color="primary" type="search" v-model="state.search">
              <template v-slot:append>
                <q-icon name="search" />
              </template>
            </q-input>
          </div>
        </div>
      </template>
      <template v-slot:body-cell-action="props">
        <q-td :props="props">
          <q-btn icon="mode_edit" @click="onEdit(props.row.o_id)" />
          <q-btn icon="delete" @click="onDelete(props.row.o_id)" />
        </q-td>
      </template>
    </q-table>
  </div>
</template>
<script setup>
import { ref } from 'vue';
import router from '../router';
import { computed, reactive, onMounted } from "vue";

const columns = ref([
  { name: 'o_id', align: 'left', label: 'o_id', field: 'o_id', sortable: true },
  { name: 'OrderDate', align: 'left', label: 'OrderDate', field: 'OrderDate', sortable: true },
  { name: 'Region', align: 'left', label: 'Regione', field: 'Region', sortable: true },
  { name: 'City', align: 'left', label: 'City', field: 'City', sortable: true },
  { name: 'Category', align: 'left', label: 'Category', field: 'Category', sortable: true },
  { name: 'Product', align: 'left', label: 'Product', field: 'Product', sortable: true },
  { name: 'Quantity', align: 'left', label: 'Quantity', field: 'Quantity', sortable: true },
  { name: 'UnitPrice', align: 'left', label: 'UnitPrice', field: 'UnitPrice', sortable: true },
  { name: 'TotalPrice', align: 'left', label: 'TotalPrice', field: 'TotalPrice' },
  { name: 'action', align: 'center', field: 'action', sortable: true },
])

const state = reactive({
  search: '',
  array: [],
  rows: []
})

const filteredData = computed(() => state.rows.filter(row => row.Product?.split(" ").join("").toLowerCase()?.includes(state.search?.split(" ").join("").toLowerCase())));

const fetchdata = () => {
  fetch("http://localhost:3000/read")
    .then(res => res.json())
    .then((result) => { state.rows = result })
}

const onEdit = (id) => {
  router.push('/update/' + id)
}

const onDelete = (id) => {
  fetch("http://localhost:3000/delete/" + id, {
    method: 'delete',
    Headers: { 'Content-Type': 'application/json' },
    redirect: 'follow'
  })
    .then(response => response.json())
    .then(result => {
      alert(result.massage)
      fetchdata()
    })
    .catch(error => console.log('error', error));
}


const onCreate = () => {
  router.push('/create')
}

onMounted(() => {
  fetchdata();
})

</script>