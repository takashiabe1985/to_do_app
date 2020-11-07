<template>
 <div id="app">
  <el-tabs v-model="activeName">
  <el-tab-pane label="ToDo" name="toDo">
    <el-table
      :data="toDos"
      style="width: 100%">
      <el-table-column
        prop="finished">
        <template v-slot="scope">
          <el-checkbox
            v-model="scope.row.finished"
            @change="updateToDo(scope.row.id, scope.row.finished)"></el-checkbox>
        </template>
      </el-table-column>
      <el-table-column
        prop="title">
      </el-table-column>
      <el-table-column
        prop="expired_at">
      </el-table-column>
      <el-table-column
        width="120">
        <template v-slot="scope">
          <el-button
            @click="destroyToDo(scope.row.id)"
            type="danger"
            icon="el-icon-delete"
            circle></el-button>
        </template>
      </el-table-column>
    </el-table>
  </el-tab-pane>
  <el-tab-pane label="終了したToDo" name="finishedToDo">終了したToDo</el-tab-pane>
</el-tabs>
 </div> 
</template>

<script>
 import axios from 'axios'
 import {reject} from 'lodash';
  export default {
  data() {
    return {
      toDos: [],
      activeName: 'toDo'
    }
  },
  created() {
    axios.get('/api/v1/to_dos')
       .then(res => {
         this.toDos = res.data
       })
  },
  methods: {
    destroyToDo(id) {
     axios.delete('/api/v1/to_dos/' + id)
      .then(res => {
        if (res.status === 200) {
         this.toDos = reject(this.toDos, ['id', id]);
        }
      });
    },
    updateToDo(id, finished) {
     axios.patch('/api/v1/to_dos/' + id, {to_do: {finished: finished}})
      .then(res => {
       if (res.status === 200) {
         console.log(res)
       }
     })
    }
  }
}
</script>