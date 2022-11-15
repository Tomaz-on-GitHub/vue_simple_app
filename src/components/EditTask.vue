<template>
  <form @submit="onSubmit"   class="add-form">
    <div class="form-control">
      <label>Task</label>
      <input type="text" v-model="text" name="text" :placeholder="text2" />
      
    </div>
    <div class="form-control">
      <label>Day & Time</label>
      <input
        type="text"
        v-model="day"
        name="day"
        placeholder="Add Day & Time"
      />
    </div>
    <div class="form-control form-control-check">
      <label>Set Reminder</label>
      <input type="checkbox" v-model="reminder" name="reminder" />

    </div>

    <input type="submit" value="Save Task" class="btn btn-block" />
     
  </form>
  
</template>

<script>
export default {
  name: 'AddTask',
  props: {
    taskToEdit:Number,
    taskToEditData:Object
    },
  data() {
    return {
      text:'',
      day: '',
      reminder: false,
    }
  },
  computed: {
    text2() {
      //console.log('task' + this.taskToEdit) 
      if (this.taskToEdit) {
        //get right task from array tasks! - first only hardcoded data

     /*   const taskToEditData = {
      "id": 1,
      "text": "Doctors Appointment",
      "day": "March 5th at 2:30pm",
      "reminder": true
    } */
      this.text = this.taskToEditData.text;
      this.day = this.taskToEditData.day;
      this.reminder = this.taskToEditData.reminder;
       return this.populateData
      } else {return 'No task to edit'}
    }
  },
  
  methods: {
    populateData(){
      return 'now data will be created'
    },
    onSubmit(e) {
      //console.log('in onsubmit function')
      e.preventDefault()
      if (!this.text) {
        alert('Please add a task')
        return
      }
      const updTask = {
        id: this.taskToEditData.id,
        text: this.text,
        day: this.day,
        reminder: this.reminder,
      }
     // console.log(newTask)
      this.$emit('edit-task', updTask)
      //clean the form
      this.text = ''
      this.day = ''
      this.reminder = false
    },
  /*
    async fetchTaskxx(id) {
      const res = await fetch(`api/tasks/${id}`)
      const data = await res.json()
      return data
    },
    */
  },
  

  }

</script>

<style scoped>
.add-form {
  margin-bottom: 40px;
}
.form-control {
  margin: 20px 0;
}
.form-control label {
  display: block;
}
.form-control input {
  width: 100%;
  height: 40px;
  margin: 5px;
  padding: 3px 7px;
  font-size: 17px;
}
.form-control-check {
  display: flex;
  align-items: center;
  justify-content: space-between;
}
.form-control-check label {
  flex: 1;
}
.form-control-check input {
  flex: 2;
  height: 20px;
}
</style>

