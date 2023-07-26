<template>
    <div class="m-4">
        <h1>Tarefas para fazer</h1>
        <p v-if="tarefas.length == 0">Você não tem nenhuma tarefa para fazer ;) </p>
        <ul class="list-group mb-1" v-for="(tarefa, i) in tarefas" :key="i">
            <li class="list-group-item d-flex justify-content-between align-items-center">
                {{ tarefa }}
                <div>
                    <button
                        type="button"
                        title="Marcar tarefa como concluida"
                        class="btn btn-sucess m-1"
                        @click="$emit('marcarConcluida')"
                    >
                        <i class="fas fa-check"></i>
                    </button>
                    <button
                        type="button"
                        title="Excluir tarefa"
                        class="btn btn-danger m-1"
                        @click="$emit('excluirTarefa')"
                    >
                        <i class="fas fa-trash"></i>
                    </button>
                </div>
            </li>
        </ul>
    </div>
</template>


<script setup>
    import { defineProps } from 'vue';


    defineProps({
        tarefas: []
    })
</script>