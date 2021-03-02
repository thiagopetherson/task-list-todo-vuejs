<template>
    <div class="task-grid">
        <template v-if="tasks.length"> <!-- O v-if está verificando se tem tarefas (Se não houver, será exibido um texto no v-else-->
            <!-- Usando o:key ali pra não ter duas tarefas com o mesmo nome -->
            <!-- Cada valor do v-for será passado para o componente Task através da prop :task -->
            <Task v-for="task in tasks" :key="task.name" :task="task" @taskDeleted="$emit('taskRemove', $event)" @taskStateChange="$emit('taskStateChange2', $event)">
            
            </Task> <!-- Poderíamos usar o event bus ao invés dessa cadeia de $emit -->
        </template>
        <p v-else class="no-task">Sua vida está em dia :)</p>
    </div>
</template>

<script>
import Task from './Task.vue'

export default {
    components: { //Adicionando o componente nesse componente atual
        Task: Task
    },
	props: {
        tasks: { type: Array, required: true}
    }
}
</script>

<style>
    .task-grid {
        display: flex;
        justify-content: center;
        flex-wrap: wrap; /* Para quebrar linha */
    }

    .task-grid .task{
       margin: 10px;
    }

    .no-task{
       color: #AAA;
       font-size: 1.7rem;
    }
</style>
