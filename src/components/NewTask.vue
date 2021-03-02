<template>
    <div class="new-task">
        <!-- O keydown é usado para quando apertarmos 'enter' executar o evento 'add'-->
        <input v-model="name" @keydown.enter="add" type="text" class="form-element" placeholder="Nova tarefa?">
        <!-- TAmbém vamos querer que o click no botão execute o evento 'add' -->
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
        //Esse método emite um evento para o componente pai (App.vue) para acrescentar uma tarefa lá no array de objetos
        //Esse método também limpa o campo (<input>) para adicionarmos uma nova tarefa
        add() { 
            this.$emit('taskAdded', { name: this.name, pending: true }) //O segundo parâmetro é a tarefa que está sendo enviado para o componente pai através do evento
            this.name = ''
       }
    }
}
</script>

<style>
    .new-task {
        margin: 35px;
    }

    .form-element {
        outline: none;
        font-size: 2em;
        border: 1px solid #FFF;
        padding: 5px 10px 8px;
        color: #FFF;
    }

    input.form-element {
        width: 500px;
        background-color: #FFF2;
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }

    button.form-element {
        border-left: none;
        background-color: #2196F3;
        border-top-right-radius: 8px;
        border-bottom-right-radius: 8px;
    }

</style>
