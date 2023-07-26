<template>

    <div class="m-4">
        <form @submit.prevent="salvarTarefa">
            <div class="mb-3">
                <label class="form-label">Nova Tarefa</label>
                <div class="d-flex flex-row">
                    <input v-model="tarefa" type="text" class="form-control me-2" placeholder="ex: Fazer dever de casa">
                    <button type="submit" class="btn btn-primary">Salvar</button>
                </div>
                {{ tarefa }}
            </div>
        </form>
    </div>

    <!-- Tarefas para fazer -->
    <ListaTarefa
        tarefas="tarefas"
        @marcarConcluida="marcarConcluida(tarefa)"
        @excluirTarefa="excluirTarefa(tarefa, tarefas)"
    />

    <!-- Tarefas Concluídas -->
    <div class="m-4">
        <h1>Tarefas Concluidas</h1>
        <p v-if="tarefasConcluidas.length == 0">Você não concluiu nenhuma tarefa :( </p>
        <ul class="list-group mb-1" v-for="(tarefa, i) in tarefasConcluidas" :key="i">
            <li class="list-group-item d-flex justify-content-between align-items-center">
                {{ tarefa }}
                <div>
                    <button
                        type="button"
                        title="refazer tarefa"
                        class="btn btn-primary m-1"
                        @click="refazerTarefa(i, tarefasConcluidas)"
                    >
                        <i class="fas fa-recycle"></i>
                    </button>
                    <button
                        type="button"
                        title="Excluir tarefa"
                        class="btn btn-danger m-1"
                        @click="excluirTarefa(i, tarefasConcluidas)"
                    >
                        <i class="fas fa-trash"></i>
                    </button>
                </div>
            </li>
        </ul>
    </div>

    <!-- Tarefas excluidas -->
    <div class="m-4">
        <h1>Tarefas Excluidas</h1>
        <p v-if="tarefasRemovidas.length == 0">Você não possui tarefas excluidas :) </p>
        <ul class="list-group mb-1" v-for="(tarefa, i) in tarefasRemovidas" :key="i">
            <li class="list-group-item d-flex justify-content-between align-items-center">
                {{ tarefa }}
                <div>
                    <button
                        type="button"
                        title="refazer tarefa"
                        class="btn btn-primary m-1"
                        @click="tarefasRemovidas(i, tarefasRemovidas)"
                    >
                        <i class="fas fa-recycle"></i>
                    </button>
                    <button
                        type="button"
                        title="Excluir tarefa"
                        class="btn btn-danger m-1"
                        @click="excluirTarefa(i, tarefasRemovidas)"
                    >
                        <i class="fas fa-trash"></i>
                    </button>
                </div>
            </li>
        </ul>
    </div>

</template>

<script setup>
import { ref, reactive } from 'vue';
import ListaTarefa from './ListaTarefa.vue';

const tarefa = ref()
const tarefas = reactive([])
const tarefasConcluidas = reactive([])
const tarefasRemovidas = reactive([])

function salvarTarefa() {
    tarefas.unshift(tarefa.value)
    tarefa.value = ""
}

function marcarConcluida(index) {
    const tarefaConcluida = tarefas.splice(index, 1)
    tarefasConcluidas.unshift(...tarefaConcluida)
}

function excluirTarefa(index, lista) {
    if(lista == tarefasRemovidas && !confirm(`Confirme a exclusão permanente da tarefa ${lista[index]}?`)) {
        return
    }
    const tarefaRemovida = lista.splice(index, 1)
    if(lista != tarefasRemovidas) {
        tarefasRemovidas.unshift(...tarefaRemovida)
    }
    
}

function refazerTarefa(index, lista) {
    const tarefa = lista.splice(index, 1)
    tarefas.unshift(...tarefa)
}

</script>