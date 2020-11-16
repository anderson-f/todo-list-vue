<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TasksProgress :progress="progress" />
		<!-- taskAdded está escutando o evento de adicionar tarefa no componente NewTask que emitiu esse evento 
		ao escutar o evento ele chama a função addTask e estou passando o parametro que recebi de NewTask pra
		função addTask - O evento por padrão é passado eu poderia explicitar isso colocando addTask($event)-->
		<NewTask @taskAdded="addTask" />
		<!-- estou passando pra TaskGrid uma lista de tarefas -->
		<!-- Aqui ao escutar o evento taskDeleted eu chamo a função deleteTask e ela já passa automaticamente o indice que foi recebido como
		evento quando o TaskGrid mandou pra cá -->
		<!-- em app escuto @taskStateChanged e chamo a função toggleTaskState  -->
		<TaskGrid :tasks="tasks"
			@taskDeleted="deleteTask" 
			@taskStateChanged="toggleTaskState" />
	</div>
</template>

<script>
import TasksProgress from './components/TasksProgress.vue'
import NewTask from './components/NewTask.vue' /* o componente que adiciona novas tarefas */
import TaskGrid from './components/TaskGrid.vue' /* a lista de tarefas */

export default {
	components: { TasksProgress, NewTask, TaskGrid },
	data() {
		return {
			tasks: [] // lista de tarefas vindas de TaskGrid.vue
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length // tamanho do array
			/* filtrando todas tasks que não estão pendentes e jogando em done o tamanho desse array gerado vai ser por exemplo
			3 tarefas done de 10 no total acima esse array vai servir pra me dizer o total de tarefas concluidas */
			const done = this.tasks.filter(t => !t.pending).length
			/* uma vez tendo a quantidade total e as tarefas concluidas só preciso calcular o percentual */
			return Math.round(done / total * 100) || 0 
			// Caso o total seja zero terei o NaN, então por isso caso eu obtenha esse valor teerei como default o 0
		}
	},
	watch: {
		/* estou observando qualquer mudança no array tasks */
		/* o watch não consegue detecar as mudanças dos objetos que estão dentro do array, ou seja
		quando o pending que faz parte do objeto que está dentro de tasks é mudado o watch não consegue escutar isso */
		tasks: { // transformo o tasks num objeto
			deep: true, // e mando ele fazer um watch profundo(vai olhar os objetos que estão dentro do array)
			handler() { // passo o handler e agora ele não olha só pros elementos, mas olha até pros atributos dos objetos
				// estou convertendo pra string pra setar no local motherfucking storage
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	},
	methods: {
		addTask(task) {
			const sameName = t => t.name === task.name // função arrow passada pro filter que verifica se a task passada já existe no array
			const reallyNew = this.tasks.filter(sameName).length == 0 // vai retornar true caso não encontre o elemento no array nenhuma vez
			/* poderia tirar o teste do if e fazer a expressão diretamente, pois o javascript já interpreta isso:
			reallyNew && this.tasks.push */
			if(reallyNew) { // verifica se posso inserir o elemento
				this.tasks.push({
					name: task.name,
					pending: task.pending || true // se task.pending não tiver nenhum valor, por default ele se torna true(está pendente)
				})
			}
		},
		deleteTask(i) {
			/* Com this.tasks.indexOf(task) obtenho o indice do elemento que desejo excluir ou executar alguma logica
			= nesse caso task é o elemento que eu quero excluir
			 */
			this.tasks.splice(i, 1)
		},
		toggleTaskState(i) {
			/* alterando o status da tarefa */
			this.tasks[i].pending = !this.tasks[i].pending
		}
	},
	created() {
		/* assim que criar a instância do vue vou pegar o array que foi armazenado no local Storage */
		const json = localStorage.getItem('tasks')
		const array = JSON.parse(json) // convertendo de json pra objetos
		this.tasks = Array.isArray(array) ? array : [] // se não for um valor correto atribui um array vazio
	}
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
