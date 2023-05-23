
<template >
  <div class="container">

    <h2 class="text-center mt-5" id="todo">TODO-LIST</h2>

    <div class="d-flex" >
      <input  type="text" v-model="task" placeholder=" Enter Task Here" class="w-100 form control">
      <button id="btn" @click="submitTask">SUBMIT</button>
    </div>
    
    <table class="table table-bordered mt-5">
      <thead>
        <tr>
          <th scope="col">Task</th>
          <th scope="col" style="width: 120px;">Status</th>
          <th scope="col" class="text-center" >Edit</th>
          <th scope="col" class="text-center">Delete</th>
        </tr>
      </thead>
      <tbody>


        <tr v-for="(task, index) in tasks" :key="index">


          <td>
            <span :class="{ 'line-through': task.status === 'finished' }">
              {{ task.name }}

            </span>

          </td>


          <td>
            <span class="pointer " @click="changeStatus(index)" :class="{
              'text-danger': task.status === 'to-do',
              'text-success': task.status === 'finished',
              'text-warning': task.status === 'in-progress',
            }">
             
            </span>

          </td>

          <td class="text-center">
            <div @click="editTask(index)">
              
              <button>Edit</button>
            </div>
          </td>

          <td class="text-center">
            <div @click="deleteTask(index)">
              
              <button>Delete</button>
            </div>
          </td>



        </tr>

      </tbody>
    </table>
  </div>
</template>

<script>
const STORAGE_KEY = 'vue-todo-app-storage';
export default {
  name: 'HelloWorld',
  props: {
    msg: String,
  },
  data() {
    return {
      task: "",

      editedTask: null,

      statuses: ["to-do", "in-progress", "finished"],

      tasks: [
        {
          name: "Mobile",
          status: "to-do",
        },
        {
          name: "Hello World",
          status: "in-orogress",
        },
        {
          name: "Computer",
          status: "finished",
        },
      ],

    };

  },
  created() {
    this.task = JSON.parse(localStorage.getItem(STORAGE_KEY) || '[]');
  },
  methods: {
    capitalizeFirstChar(str) {
      return str.charAt(0).toUppercase() + str.slice(1);
    },
    // change index
    changeStatus(index) {
      let newIndex = this.statuses.indexOf(this.tasks[index].status);
      if (++newIndex > 2) newIndex = 0;
      this.tasks[index].status = this.statuses[newIndex];
    },
    // delete tasks

    deleteTask(index) {
      this.tasks.splice(index, 1);
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks));
    },
    // eidt task

    editTask(index) {
      this.task = this.tasks[index].name;
      this.editedTask = index;
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks));
    },
    submitTask() {
      if (this.task.length === 0) return;
      if (this.editedTask != null) {
        this.tasks[this.editedTask].name = this.task;
        this.editedTask = null;
      } else {

        this.tasks.push({
          name: this.task,
          status: "todo",
        });
      }
      this.task = "";
      localStorage.setItem(STORAGE_KEY, JSON.stringify(this.tasks));
    },
  },

};

</script>
<style>
#btn{
  color:red;
  background-color:black ;
  width: 90px;
  height: 50px;
}
#todo{
  
margin-top: 150px !important;
margin-bottom: 50px;
}
</style>