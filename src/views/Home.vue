<template>
  <AddTask v-show="showAddTask" @add-task="addTask" 
  />
  <EditTask v-show="showEditTask" @edit-task="editTask2"
  :taskToEdit="taskToEdit" 
  :taskToEditData="taskToEditData"
  />
  <Tasks
    @toggle-reminder="toggleReminder"
    @delete-task="deleteTask"
    @edit-task="editTask1"
    :tasks="tasks"
  />
</template>

<script>
import Tasks from '../components/Tasks'
import AddTask from '../components/AddTask'
import EditTask from '../components/EditTask'


export default {
  name: 'Home',
  props: {
    showAddTask: Boolean,
    showEditTask: Boolean,
    
  },
  components: {
    Tasks,
    AddTask,
    EditTask
  },
  data() {
    return {
      tasks: [],
      taskToEdit:0,
      taskToEditData: {},
    }
  },
  methods: {
    async addTask(task) {
      const res = await fetch('api/tasks', {
        method: 'POST',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(task),
      })
      const data = await res.json()
      this.tasks = [...this.tasks, data]
    },

  editTask1(id) {  //put data to modify on the form
    console.log('in home -edit task id:' + id)
    //const taskToEdit2 = await this.fetchTask(id) //get data
    //console.log('edit data:' + taskToEdit.text)
    if (id) {
    this.taskToEditData = this.tasks.filter((task) => (task.id === id));
    //this.taskToEditData = this.taskToEditData;
    //const updTask2 = this.taskToEditData[0];   
    //console.log('in home -edit task: ' + updTask2.text)
    this.taskToEditData = this.taskToEditData[0]; 
    //console.log('in home -edit task2: ' + this.taskToEditData.text)
    //const taskToToggle = await this.fetchTask(id)
    this.taskToEdit = id;
    //console.log('in home -edit task:' + this.taskToEdit)
    //show the input form
    this.$emit('show-form-for-edit');
    }
  },

  async editTask2(updTask) {   //put modified data in the DB and string
    //console.log(NewTask)
    const res = await fetch(`api/tasks/${updTask.id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      })
      const data = await res.json()
      console.log(data)
      
      this.tasks = this.tasks.map((task) =>
        task.id === data.id ? { ...data } : task
      )
      //close the EditTask frame
      this.$emit('show-form-for-edit');
      
  },

    async deleteTask(id) {
      if (confirm('Are you sure?')) {
        const res = await fetch(`api/tasks/${id}`, {
          method: 'DELETE',
        })
        res.status === 200
          ? (this.tasks = this.tasks.filter((task) => task.id !== id))
          : alert('Error deleting task')
      }
    },
    async toggleReminder(id) {
      const taskToToggle = await this.fetchTask(id)
      const updTask = { ...taskToToggle, reminder: !taskToToggle.reminder }
      //console.log('to toggle:' + taskToToggle.text)
      const res = await fetch(`api/tasks/${id}`, {
        method: 'PUT',
        headers: {
          'Content-type': 'application/json',
        },
        body: JSON.stringify(updTask),
      })
      const data = await res.json()
      this.tasks = this.tasks.map((task) =>
        task.id === id ? { ...task, reminder: data.reminder } : task
      )
    },
    async fetchTasks() {
      const res = await fetch('api/tasks')
      const data = await res.json()
      return data
    },
    async fetchTask(id) {
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
    },
  },
  async created() {
    this.tasks = await this.fetchTasks()
  },
  emits: ['show-form-for-edit'],
}
</script>