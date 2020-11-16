<template>
    <div class="new-task">
        <!-- defini form-element pro input e pro button, pois teremos alguns estilos que serão comuns aos dois -->
        <input v-model="name" @keydown.enter="add" type="text"
            class="form-element" placeholder="Nova tarefa?">
        <button class="form-element" @click="add">+</button>
    </div>
</template>

<script>
export default {
    data() {
        return {
            name: ''
        }
    },
    methods: {
        add() {
            /* estou emitindo um evento pro pai toda vez que adiciono algum elemento(seja enter ou clique)*/
            this.$emit('taskAdded', { name: this.name })
            this.name = '' // preciso limpar o input
        }
    }
}
</script>

<style>
    .new-task {
        margin: 35px; /* dar um espaço pra barra e entre a grid de tarefas */
    }
    /* pro input e pro button */
    .form-element {
        outline: none;
        font-size: 2rem;
        border: 1px solid #FFF;
        padding: 5px 10px 8px;
        color: #FFF;
    }

    /* espeficiamente pro input */
    input.form-element {
        width: 500px;
        background: #FFF2;
        border-top-left-radius: 8px; /* definindo as bordas pretinhas do canto  */
        border-bottom-left-radius: 8px;/* definindo as bordas pretinhas do canto */
    }

    /* especificamente pro button */
    button.form-element {
        border-left: none;
        background-color: #2196F3;
        border-top-right-radius: 8px; 
        border-bottom-right-radius: 8px;
    }
</style>
