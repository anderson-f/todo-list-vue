<template>
<!-- task representa uma tarefa singular -->
    <!-- quem precisa receber o evento é o app, mas quem é o pai desse componente é o TaskGrid.vue então quem vai receber
    esse evento será ele -->
    <!-- taskStateChanged tem a mesma logica do taskDeleted, vou emitir um evento aqui pro grid -->
    <div @click="$emit('taskStateChanged', task)"
        class="task" :class="stateClass">
        <!-- Cada task tem uma span que tem esse x associado, quando eu clico no X disparo o evento referente aquela task especifica
        escuto isso em TaskGrid e lá emito um evento pra app passando o indice da task que foi notificada pelo evento -->
        <!-- Se eu não associar ao click o .stop terei um problema em alguns casos em que o evento de deletar será chamado antes do evento de 
        mudar o status da tarefa, dessa forma a aplicação vai tentar trabalhar com o pending de uma tarefa que já foi excluida, pra acabar
        com isso associoamos o stop ao evento de click dessa forma ao ser associonado esse evento ele não escuta mais evento de click daquele
        elemento é similar ao off do jQuery -->
        <span @click.stop="$emit('taskDeleted', task)" class="close">x</span><!-- quem vai excluir a tarefa -->
        <p>{{ task.name }}</p>
    </div>
</template>

<script>
export default {
    props: {
        task: { type: Object, required: true } // estou recebendo essa task vinda de TaskGrid
    },
    computed: {
        /* Uma propriedade computada, vai retornar true pra uma classe ou pra outra */
        stateClass() {
            return {
                pending: this.task.pending,
                done: !this.task.pending
            }
        }
    }
}
</script>

<style>
    .task {
        position: relative; /* coloquei relative pro absolute do X considerar a task como limite */
        box-sizing: border-box; /* vai dizer que ele vai considerar como tamanho da caixa tambem o padding e aborda e não apenas o conteudo*/
        width: 350px; /* altura */
        height: 150px; /* largura */
        padding: 10px;
        border-radius: 8px;
        font-size: 2rem;
        font-weight: 300;
        cursor: pointer; /* faz com que apareca o dedinho do mouse dizendo que é uma area clicável */
        user-select: none; /* o usuário não consegue selecionar o texto */
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pending {
        border-left: 12px solid #B73229; /* faz aquela bordinha nice mais escura no canto */
        background-color: #F44336;
    }

    .done {
        color: #DDD; /* deixando o texto mais claro pra dar menos importancia */
        border-left: 12px solid #0A8F08;
        background-color: #4CAF50;
        text-decoration: line-through;/* risca o texto */
    }

    .pending .close {
        background-color: #B73229; /* background do X de fechar quanto a tarefa ta pendente */
    }

    .done .close {
        background-color: #0A8F08; /* background do X de fechar quanto a tarefa ta concluida */
    }
    /* O position absolute ele busca o primeiro item que possui position relative e se posiciona de acordo com ele. 
    Como task não tem, ele sobe outro nível e assim sucessivamente até chegar no body. Sacou? */
    .close {
        /* o absoluto se refere a pagina inteira e pra eu applicar isso só pro elemento que eu quero trabalhar 
        dai coloco o pai dele como relative*/
        position: absolute; /* faz com que eu possa posicionar no lugar que eu quero */
        right: 10px; /* esse right e top joga o x pra direita e pro topo */
        top: 10px;
        font-size: 0.9rem;
        font-weight: 600; /* fonte fica um pouco mais grossa */
        /* essas três propriedades que fazem ficar circular */
        height: 20px;
        width: 20px;
        border-radius: 10px;
        /* fim */
        /* essas duas propriedades fazem com que o x vá pro centro do circulo */
        display: flex;
        justify-content: center;
        /* fim */
    }
</style>
