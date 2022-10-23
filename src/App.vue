<template>
    <div>
        <h1 class="bg-info text-white text-center py-3">{{ title }}</h1>

        <div class="row">
            <div
                class="col-sm-10 offset-sm-1 col-md-8 offset-md-2 col-lg-6 offset-lg-3"
            >
                <div class="px-3">
                    <div class="my-4 d-flex gap-1">
                        <input
                            type="text"
                            class="form-control"
                            placeholder="Enter task ..."
                            v-model="newTask"
                            @keyup.enter="addTask"
                        />
                        <button class="btn btn-primary px-4" @click="addTask">
                            Add
                        </button>
                        <button class="btn btn-danger px-3" @click="deleteTask">
                            Delete
                        </button>
                    </div>

                    <div class="row">
                        <h3 class="col">Tasks</h3>
                        <h3 class="col-2">Done</h3>
                    </div>

                    <div
                        class="alert alert-warning text-center"
                        v-if="!tasks.length"
                    >
                        There is no task yet !
                    </div>

                    <div
                        class="row"
                        v-for="(task, index) in filterTask"
                        v-bind:key="index"
                    >
                        <div class="col" :class="task.status ? doneStyle : ''">
                            {{ task.action }}
                        </div>
                        <div class="col-2">
                            <input
                                type="checkbox"
                                v-model="task.status"
                                @change="updateTask"
                            />
                        </div>
                    </div>

                    <div
                        class="row mt-4 mx-auto px-3 py-2 bg-success text-white align-items-center"
                    >
                        <h5 class="col p-0 m-0">Hide Completed Tasks</h5>
                        <input
                            type="checkbox"
                            class="col-2"
                            v-model="hideCompleted"
                        />
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",

    mounted() {
        let myLists = localStorage.getItem("myTasks");
        if (myLists != null) {
            this.tasks = JSON.parse(myLists);
        }
    },

    data: () => ({
        title: "My Todo Lists",
        newTask: "",
        hideCompleted: false,
        tasks: [],
        doneStyle: "text-decoration-line-through",
    }),

    methods: {
        addTask() {
            if (!this.newTask) {
                return alert("Please enter task !");
            }

            this.tasks.push({
                action: this.newTask,
                status: false,
            });

            localStorage.setItem("myTasks", JSON.stringify(this.tasks));
            this.newTask = "";
        },
        updateTask() {
            localStorage.setItem("myTasks", JSON.stringify(this.tasks));
        },
        deleteTask() {
            this.tasks = this.tasks.filter((task) => !task.status);
            localStorage.setItem("myTasks", JSON.stringify(this.tasks));
        },
    },

    computed: {
        filterTask() {
            return this.hideCompleted
                ? this.tasks.filter((task) => !task.status)
                : this.tasks;
        },
    },
};
</script>
