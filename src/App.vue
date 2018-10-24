<template>
    <div id="app">
        <div id="header-elements">
            <h1>Vue.Js Todo List By Robin</h1>
            <h3>Nombre de tâche : {{ allTasks.length }}</h3>
            <input id="input-task"
            placeholder="Ajouter une nouvelle tâche ?"
            v-model="newTaskTitle"
            @keyup.enter="addNewTask">
            <button @click="addNewTask">Ajouter la tâche</button>
        </div>
        <p>{{ errorMessage }}</p>
        <div v-for="(task, index) in allTasks">
            <transition name="fade">
                <div id="my-list" :class="{ 'seen': task.read }">
                    <input type="checkbox" id="checkbox" v-model="task.read">
                    <span>{{ task.title }}</span>
                    <img src="./assets/delete.png" width="20" id="delete"
                    @click="deleteItem(index)">
                </div>
            </transition>
        </div>
    </div>
</template>

<script>

export default {
    name: 'TodoList',
    data: function () {
        return {
            allTasks: [],
            newTaskTitle: "",
            errorMessage: ""
        }
    },
    mounted() {
        if (localStorage.allTasks) {
            this.allTasks = JSON.parse(localStorage.allTasks);
        }        
    },
    watch: {
        allTasks(NewAllTasks) {
            localStorage.allTasks = JSON.stringify(NewAllTasks);
        }
    },
    methods: {
        addNewTask: function () {
            this.errorMessage = ""
            if (!this.newTaskTitle) {
                this.errorMessage = "Saisissez un nom pour votre tâche"
            } else {
                const task = {
                    title: this.newTaskTitle,
                    read: false  
                }
                this.allTasks.push(task);
                this.newTaskTitle = "";
            }
        },
        deleteItem: function (index) {
            this.allTasks.pop(index);
        },
    }
};
</script>

<style>

#app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 60px;
}

h1, p, #header-elements, #my-list {
    text-align: center;
}

p {
    color: red;
}

button {
    background-color: #55be8d;
    color: white;
    font-weight: bold;
    padding: 8px;
    font-size: 17px;
}

#input-task {
    padding: 8px;
    font-size: 17px;
    width: 49%;
}

#my-list {
    background-color: white;
    list-style-type:none;
    margin-top: 1em;
    box-shadow: 0px 0px 8px;
    border-radius: 17px;
    width: 50%;
    margin-left: auto;
    margin-right: auto;
    padding-bottom: 3%;
    padding-top: 3%;
}

#my-list:hover {
    box-shadow: 0px 0px 8px blue;
}

#delete {
    float: right;
    cursor: pointer;
    margin-right: 1em;
}

#checkbox {
    float: left;
    margin-left: 3em;
}

.seen {
    opacity: 0.4;
}

</style>
