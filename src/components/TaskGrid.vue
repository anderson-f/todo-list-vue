<template>
    <!-- task-grid é a lista de tarefas -->
    <div class="task-grid">
        <!-- verifica se tasks.lenght está maior do que zero para exibir uma mensagem diferente
        caso não exista nenhuma tarefa -->
        <template v-if="tasks.length">
            <!-- aqui eu percorro a lista de tasks que recebi de app e jogo pra dentro de task
            a tarefa especifica -->
            <!-- estou escutando o evento emitido no clique do X da task e quando escuto esse evento emito outro pro pai de TaskGrid que é app
            passando o indice do elemento que quero remover do array -->
            <Task v-for="(task, i) in tasks" :key="task.name"
                @taskDeleted="$emit('taskDeleted', i)"
                @taskStateChanged="$emit('taskStateChanged', i)"
                :task="task"></Task>
                <!-- aqui em grid escuto o  @taskStateChanged e emito um evento pra app -->
        </template>
        <p v-else class="no-task">Sua vida está em dia :)</p>
    </div>
</template>

<script>
import Task from './Task.vue'

export default {
      /* Em props registrei um atributo que eu pego de App que é a lista de tasks, registrei o componente Task em components
       e lá em cima eu passo pra task a tarefa especifica*/
    components: { Task },
    props: {
        tasks: { type: Array, required: true } // estou recebendo de app a lista de tasks
    }
}
</script>

<style>
    .task-grid {
        display: flex; /* padrão em linha */
        justify-content: center;
        flex-wrap: wrap; /* permite quebrar linha */
    }

    .task-grid .task {
        margin: 10px;
    }

    .no-task {
        color: #AAA;
        font-size: 1.7rem;
    }
</style>
