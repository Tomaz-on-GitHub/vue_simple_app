<template>
  <div @dblclick="$emit('toggle-reminder', task.id)"
  :class="[task.reminder ? 'reminder': '','task']"> 
     
    <h3>
      {{ task.text }}

      <i @click="editTask(task.id)" class="fa fa-pen fa-1g"></i>
      <i @click="onDelete(task.id)" class="fas fa-times"></i>
    </h3>
    <p>{{ task.day }}</p>

  </div>

</template>

<script>


export default {
   name: "Task",
   props: {
    task: Object,
   },
   methods: {
    onDelete(id) {
        //console.log('delete' + id);
        this.$emit('delete-task',id)
    },
    editTask(id) {
      //console.log('edit' + id);
      this.$emit('edit-task',id)
    }
   }
}
</script>

<style scope>
.fa {
  margin-left: auto;
  padding-right:5px;
}

.fas {
  color: red;
}
.task {
  background: #f4f4f4;
  margin: 5px;
  padding: 10px 20px;
  cursor: pointer;
}
.task.reminder {
  border-left: 5px solid green;
}
.task h3 {
  display: flex;
  align-items: center;
  justify-content: left;
}
</style>
