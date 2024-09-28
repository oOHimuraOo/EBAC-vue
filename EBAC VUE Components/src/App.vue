<script setup>
import { reactive } from 'vue';
  
  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        concluido: false
      },
      {
        titulo: 'Estudar SASS',
        concluido: false
      },
      {
        titulo: 'Estudar Less',
        concluido: true
      },
      {
        titulo: 'Estudar VUE',
        concluido: false
      },
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa=>!tarefa.concluido)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa=>tarefa.concluido)
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes()
      case 'finalizadas':
        return getTarefasFinalizadas()
      default:
        return estado.tarefas
    }
  }

  const cadastraTarefa = () => {
    const novaTarefa = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(novaTarefa)
    estado.tarefaTemp = ''
  }

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <input :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" required type="text" class="form-control" placeholder="Digite aqui uma nova tarefa!">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">
            Cadastrar
          </button>
        </div>
        <div class="col-md-2">
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="(tarefa,index) in getTarefasFiltradas()" :key="index">
        <input @change="evento => tarefa.concluido = evento.target.checked" :checked="tarefa.concluido" :id="tarefa.titulo" type="checkbox">
        <label :for="tarefa.titulo" class="ms-3" :class="{ done: tarefa.concluido}">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
  .done {
    text-decoration: line-through;
  }
</style>
