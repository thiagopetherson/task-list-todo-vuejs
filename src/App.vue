<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TasksProgress :progress="progress"></TasksProgress>
		<NewTask @taskAdded="addTask($event)"></NewTask>
		<!-- Nesse componente abaixo tem dois eventos sendo tratados. O de deletar uma tarefa e o de mudar o status -->
		<TaskGrid v-bind:tasks="tasks" @taskRemove="deleteTask($event)" @taskStateChange2="toggleTaskState($event)">

		</TaskGrid> <!-- Renderizando o Componente e passando um valor para sua prop -->
	</div>
</template>

<script>
import TasksProgress from './components/TasksProgress.vue' //Importando o componente
import NewTask from './components/NewTask.vue' //Importando o componente
import TaskGrid from './components/TaskGrid.vue' //Importando o componente


export default {
	components: { //Adicionando o componente nesse componente atual
		NewTask: NewTask,
		TaskGrid: TaskGrid,
		TasksProgress: TasksProgress
	},
	data () {
		return {
			tasks: [] //Lista de Tarefas (fixa)	
		}
	},
	computed: { //Essa propriedade computada está sempre atualizando a barra de progresso de acordo com as tarefas existentes
		progress() {
			//Vendo quantas tarefas tem
			const total = this.tasks.length 
			//A função anônima dentro do filter pegará todo mundo que não está pendente e retornará o tamanho desse array (quantas funções não estão pendentes)
			const done = this.tasks.filter(t => !t.pending).length 
			//Calculando o percentual de concluídas
			return Math.round(done / total * 100) || 0 //Caso 'total' seja 0 então será gerado um NaN, por isso usamos o OU (||) ali
		}
	},
	watch: {
		tasks: {
			deep: true,
			handler() {
				//Abaixo, estamos pegando a lista de tasks, convertendo para string e setando (armazenando) em LocalStorage a partir da chave 'tasks'	
				localStorage.setItem('tasks', JSON.stringify(this.tasks)) 
			}
		}
	},
	created() { //Esse método do ciclo de vida pegará as informações do LocalStorage e setará na aplicação
		
		//Pegando o valor do LocalStorage
		const json = localStorage.getItem('tasks') 

		//Uma vez que temos o nosso json em formato String, nós podemos usar a função JSON.PARSE para gerar isso um array e setar no estado do nosso componente
		const array = JSON.parse(json)

		//Verificando se o resultado vindo do LocalStorage foi um array, então setamos nossa propriedade
		//Se não for array (else) então setaremos a propriedade com um array vazio
		if(Array.isArray(array)) {
			this.tasks = array 
		} else {
			this.tasks = []
		}
	},
	methods: {
		addTask(task) { //Adiciona a tarefa que vem do input do componente NewTask

			//Verificando se a Tarefa já existe
			const sameName = t => t.name === task.name //Função anônima que recebe um parâmetro e verifica o parâmetro é igual a um valor

			//Fazemos um filter() para percorrer todo o Array e filtrar todas as tasks que tem o mesmo nome, e se o array final for vazio...
			//...significa que o array final será 0 e a expressão será verdadeira
			const reallyNew = this.tasks.filter(sameName).length == 0 //Poderia ser feito de uma outra forma
			
			//Se não houver tarefa com esse nome, então faremos a criação dela
			if(reallyNew) {
				this.tasks.push(task)
			} else {
				alert('Essa atividade já existe')
			}			
		},
		deleteTask(task) { /* Excluir uma tarefa */
			const index = this.tasks.indexOf(task) //Pegando a posição no array que aquela tarefa está
			this.tasks.splice(index, 1) //Removendo o elemento do array					
		},
		toggleTaskState(task) { //Trocando o status de uma tarefa
			const index = this.tasks.indexOf(task) //Pegando a posição no array que aquela tarefa está
			this.tasks[index].pending = !this.tasks[index].pending //Se estiver pendente é colocado false, se não estiver pendente é colocado verdade				
		}
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
