<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import Listadetarefas from './components/Listadetarefas.vue';

  const estado = reactive({
    filtro: "todas",
    tarefaTemp: "",

    tarefas: [
      {
        titulo: "Estudar ES6",
        finalizada: false,
      },
      {
        titulo: "Estudar SASS",
        finalizada: true,
      },
      {
        titulo: "Ir para academia",
        finalizada: false,
      },
      {
        titulo: "Estudar Inglês",
        finalizada: false,
      }
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada);
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada);
  }

  const getTarefasFiltradas = () => {
    const { filtro } = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      
      case 'finalizadas':
        return getTarefasFinalizadas();

      default:
        return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }

    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = "";
  }
</script>

<template>

  <div class="container">
    <Cabecalho :tarefasPendentes="getTarefasPendentes().length" />
    <Formulario :trocarFiltro="evento => {estado.filtro = evento.target.value}" :tarefaTemp="estado.tarefaTemp" :editaTarefaTemp="evento => { estado.tarefaTemp = evento.target.value }" :cadastraTarefa="cadastraTarefa"/>
    <Listadetarefas :tarefas="getTarefasFiltradas()"/>
  </div>
  
</template>