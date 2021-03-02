<template>
    <!-- O :class adiciona uma classe nesse elemento -->
    <div class="task" :class="stateClass" @click="$emit('taskStateChange', task)"> 
        <!-- Usamos o ".stop" para o evento não se propagar, ou seja, quando clicar naquele evento ali, não ativar o evento da div também.
        Já que o span está dentro da div -->
        <span @click.stop="$emit('taskDeleted', task)" class="close">x</span> 
        <p>{{ task.name }}</p>
    </div>
</template>

<script>
export default {
	props: {
        task: { type: Object, required: true}
    },
    computed: {
        stateClass() { //Criamos esse propriedade computada para calcular qual é o tipo da classe CSS que vamos aplicar
            return {
                //Aqui fazer que se a task estiver pendente, então criaremos uma classe 'pending', caso contrário criaremos uma classe 'done'
                pending: this.task.pending, 
                done: !this.task.pending
            }
        }
    }  
}
</script>

<style>
    .task {
        position: relative; /* Colocando isso por causa do botão de close */
        box-sizing: border-box;
        width: 350px;
        height: 150px;
        padding: 10px;
        border-radius: 8px;
        font-size: 2rem;
        font-weight: 300;
        cursor: pointer;
        user-select: none; /* Não permite que o usuário seleciona o texto */
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .pending { /* Classe da tarefa pendente */
        border-left: 12px solid #B73229;
        background-color: #F44336;
    }

    .done { /* Classe da tarefa concluída */
        color: #DDD;
        border-left: 12px solid #0A8F08;
        background-color: #4CAF50;
        text-decoration: line-through; /* Risca o texto */
    }

    .pending .close { /* Estilizando o 'X' do pending */
        background-color: #B73229;
    }

    .done .close { /* Estilizando o 'X' do done */
        background-color: #0A8F0B;
    }

    .close {
        position: absolute;
        right: 10px;
        top: 10px;
        font-size: 0.9rem;
        font-weight: 600;
        width: 20px;
        height: 20px;
        border-radius: 10px;
        display: flex;
        justify-content: center;
    }

</style>
