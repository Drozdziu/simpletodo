<template>
    <div id="list">
        <input type="text" v-model="task">
        <button @click="addTask" id="add">Add task</button>
        <ul v-if="tasks.length > 0">
            <li v-for="(task, index) in tasks" :key="index">
                <span>{{ task.content }}</span>
                <button v-if="!tasks[index].isDone" @click="doTask(index, task.content)" id="do">Do</button>
                <p v-else> ✔ </p>
                <button @click="deleteTask(index++)" id="delete">Delete</button>
            </li>
        </ul>
        <p v-else>No tasks available 🥳</p>
    </div>
</template>

<script>
export default {
    name: 'ListComponent', // nazwa komponentu
    mounted() { // Gdy komponent trafił do DOM
		this.tasks = this.fillArray()
	},
    data() { // zmienne
        return {
            task: '',
            tasks: [],
            doneTasks: [],
            taskObject: {
                content: String,
                isDone: Boolean
            }
        }
    },
    methods: { // metody
        fillArray(){
            let arr = [];
            let index = 1;
            for(let i = 0; i<localStorage.length; i++){
                if (localStorage.key(`task-${index}`).startsWith('task')) arr.push(JSON.parse(localStorage.getItem(`task-${index}`)))
                index++;
            }
            return arr;
        },
        addTask() {
            if (this.task.trim()) { // czy nie ma głupich spacji
                this.doneTasks.push(false);
                const newTask = {
                    content: this.task,
                    isDone: false
                };
                this.tasks.push(newTask);
                localStorage.setItem(`task-${this.tasks.length}`, JSON.stringify(newTask));
                this.task = '';
            }
        },
        doTask(i, text) {
            this.taskObject.content = text;
            this.taskObject.isDone = true;
            this.tasks[i] = this.taskObject;
            localStorage.setItem(`task-${i+1}`, JSON.stringify(this.taskObject));
        },
        deleteTask(i){ // poprawiona przez copilota
            this.tasks.splice(i, 1);
            this.doneTasks.splice(i, 1);
            const tasksLength = this.tasks.length + 1;
            for (let j = i + 1; j < tasksLength; j++) {
                const item = localStorage.getItem(`task-${j + 1}`);
                if (item !== null) localStorage.setItem(`task-${j}`, item);
            }
            localStorage.removeItem(`task-${tasksLength}`);
        }
        // deleteTask(i){ moja metoda (nie działała)
        //     this.tasks.splice(i, 1);
        //     this.doneTasks.splice(i, 1);
        //     for(let j = i; j<=localStorage.length; j++){
        //         let newIndex = j - 1;
        //         const item = localStorage.getItem(`task-${newIndex}`) // 3 
        //         localStorage.setItem(`task-${newIndex}`, item) // 3
        //     }
        //     i+=1;
        //     localStorage.removeItem(`task-${i}`);
        // }
    }
}
</script>

<style scoped>
#list{
    font-weight: 900;
}
#add{
    min-width: 200px;
}
ul{
    border: 1px solid black;
    border-radius: 10px;
    box-shadow: 0 6px 12px rgba(0, 0, 0, 0.1);
}
li {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 1px;
    /* odstęp między elementami */
}

span {
    display: flex;
    flex: 0.1;
    border: 1px black solid;
    border-radius: 10px;
    min-height: 35px;
    justify-content: center; /* poziome wyśrodkowanie */
    align-items: center; 
    font-weight: 600;
    margin: 10px;
    padding: 5px;
}
p{
    margin-left: 23px;
    margin-right: 23px;
}
input{
    padding: 8px;
    font-weight: 700;
    border-radius: 10px;
    border: 3px solid #42b983;
}
input:focus{
    outline: none;
}
button {
    background-color: #d2f3e4;
    transition: 0.5s ease background-color;
    transition: 0.3s linear transform;
    border: 1px solid #42b983;
    font-weight: 700;
    padding: 8px;
    border-radius: 10px;
    margin: 10px;
    max-height: 40px;
}
button:hover{
    cursor: pointer;
    background-color: #82fdc4;
}
button:active{
    background-color: #009b55;
    transform: scale(0.95);
}
#do:hover{
    background-color: #42b983;
}
#delete:hover{
    background-color: red;
}
</style>
