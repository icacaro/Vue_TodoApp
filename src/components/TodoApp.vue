<template>
  <div class="container">
    <h2 class="text-center mt-5">My vue Todo App</h2>

    <div class="d-flex">
      <input v-model="tarefa" type="text" placeholder="Inserir Tarefa" class="form-control">
      <button @click="incluirTarefa" class="btn btn-warning rounded-0">Incluir</button>
    </div>

    <!--tabela-->
    <table class="table table-bordered mt-4">
      <thead>
        <tr>
          <th scope="col">Tarefa</th>
          <th scope="col">Situação</th>
          <th scope="col" class="text-center">#</th>
          <th scope="col" class="text-center">#</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(tarefa, index) in tarefas" :key="index">
          <td>
            <span :class="{'feito': tarefa.status === 'feito'}" >{{tarefa.name}}</span>
          </td>
          <td>
            <span @click="trocarStatus(index)" class="pointer"
            :class="{'text-danger': tarefa.status === 'A fazer',
            'text-warning': tarefa.status === 'em progresso',
            'text-success': tarefa.status === 'feito'
          }"
            >
              {{tarefa.status}}
            </span>
          </td>
          <td>
            <div class="text-center" @click="editarTarefa(index)">
              <span class="fa fa-pen"></span>
            </div>
          </td>
          <td>
            <div class="text-center" @click="deletarTarefa(index)">
              <span class="fa fa-trash"></span>
            </div>
          </td>
        </tr>

      </tbody>
    </table>




  </div>
</template>

<script>
export default {
  name: 'TodoList',
  props: {
    msg: String
  },

  data(){
    return {
      tarefa: '',
      tarefaEditada: null,
      tiposDeStatus: ['a fazer', 'feito', 'em progresso'],

      tarefas: [
        {
          name: 'Estudar Vuejs',
          status: 'em progresso'
        },
      ]
    }
  },

  methods: {
    incluirTarefa(){
      if(this.tarefa.length === 0) return;

      if(this.tarefaEditada === null){
      this.tarefas.push({
        name: this.tarefa,
        status: 'a fazer'
      });
      } else {
        this.tarefas[this.tarefaEditada].name = this.tarefa;
        this.tarefaEditada = null;
      }

      this.tarefa ='';
    },

    deletarTarefa(index){
      this.tarefas.splice(index, 1);
    },

    editarTarefa(index){
      this.tarefa = this.tarefas[index].name;
      this.tarefaEditada = index;
    },

    trocarStatus(index){
      let newIndex = this.tiposDeStatus.indexOf(this.tarefas[index].status);
      if(++newIndex > 2) newIndex =0;
      this.tarefas[index].status = this.tiposDeStatus[newIndex];
    },


  }

}
</script>

<style scoped>
.pointer{
  cursor: pointer;
}
.feito{
  text-decoration: line-through;
}
</style>

