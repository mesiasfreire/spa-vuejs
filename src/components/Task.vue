<template>
  <div>
      <form @submit.prevent="storeNewTask" method="post">
        <label for="">Nome</label>
        <input type="text" name="name" id="" v-model="dataForm.name">

        <label for="">Descricao</label>
        <input type="text" v-model="dataForm.description">

        <input type="submit" value="Cadastrar">
      </form>

      <table>
          <thead>
              <th>Id</th>
              <th>Nome</th>
              <th>Descrição</th>
          </thead>
          <tbody>
              <tr v-for="task in allTasks[0]" :key="task.id">
                  <td>{{task.id}}</td>
                  <td>{{task.name}}</td>
                  <td>{{task.description}}</td>
                  <td><a  @click="editTask(task.id)"> Editar</a></td>
                  <td><a  @click="delTask(task.id)"> Deletar</a></td>
              </tr>
          </tbody>

      </table>
  </div>
</template>

<script>

import axios from 'axios';
export default {
    name:'task',

    data(){
        return {
            dataForm:{
                name:'',
                description:''
            },

            allTasks:[]
        }
    },
    methods:{

     async storeNewTask(){
            
            const response = await axios.post('http://laravel.api/api/task',{
                name:this.dataForm.name,
                description:this.dataForm.description
            })
            .then(resp=>{
                console.log(resp);
            })
            .catch(err=>{
                console.log(err);
            });
        },

         async getTasks(){
            const resp = await axios.get('http://laravel.api/api/task')
            .then(resp =>{
                if(resp.data){
                    this.allTasks.push(resp.data);                 
                }
            }).catch(err=>{
                console.log(err);
            })
        },
        delTask(v){
            console.log(v);

        },
        editTask(v){
            console.log(v);
        }
    },

    created(){
       this.getTasks();

    },

    watched:{
        allTasks(val){
            console.log(val);
        }
    }
    
}
</script>

<style>

</style>