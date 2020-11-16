<template>
    <div class="tasks-progress">
        <span class="progress-value">{{ progress }}%</span>
        <div class="progress-bar" :style="{width: progress + '%'}"></div>
    </div>
</template>

<script>
export default {
    props: {
        progress: { type: Number, default: 0 } // se não passar nada o progresso vai ser 0
    }
}
</script>

<style>
    .tasks-progress {
        position: relative; /* preciso usar posição relativa pq no filho irei usar abosluta */
        width: 80%;
        border: 1px solid #FFF;
        color: #FFF;
        border-radius: 8px;
        margin-bottom: 15px;
        display: flex;
        flex-direction: column;
        align-items: center;
    }

    .progress-bar {
        position: absolute;
        border-radius: 8px;
        height: 100%; /* preenche toda a div que estou envolvido */
        background-color: #4CAF50;
        align-self: flex-start; /* inicia da esquerda e não do centro o processo de enchimento da barra de progresso */
    }

    .progress-value {
        z-index: 1; /* faz com que o valor dentro da barra que mostra a porcentagem não seja sobrescrito pelo background */
        font-size: 1.5rem;/* aumentei a fonte */
    }
</style>
