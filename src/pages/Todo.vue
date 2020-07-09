<template>
  <q-page class="bg-grey-3 column">

    <div class="row q-pa-sm bg-primary">
      <q-input
      @keyup.enter="addTask"
      class="col"
      square
      filled
      bg-color = "white"
      v-model="newTask" 
      placeholder="Add Task"
      dense>
        
        <!-- <template v-slot:before>
          <q-icon name="event" />
        </template>

        <template v-slot:hint>
          Field hint
        </template> -->

        <template v-slot:append>
          <q-btn
          @click="addTask" 
          round 
          dense 
          flat 
          icon="add" />

        </template>
      </q-input>
    </div>
    
    <q-list class="bg-white" 
     separator
     bordered>
      <q-item
        v-for="(task,index) in tasks"
        :key = "task.title"
        @click="task.done =!task.done"
        :class="{ 'done bg-blue-1' : task.done }" 
        clickable
        v-ripple>
        <!-- here, class will be 'done' when task.done is true -->
        <q-item-section avatar>
          <q-checkbox 
          v-model="task.done"  
          class="no-pointer-events"
          color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.title}}</q-item-label>
        </q-item-section>
         <q-item-section
         v-if="task.done" side>
          <q-btn 
          @click.stop = "deleteTask(index)"
          flat 
          dense 
          round 
          color="primary" 
          icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
  <div 
   v-if = "!tasks.length"
  class="no-tasks absolute-center">
    <q-icon
      name="check"
      size="100px"
      color="primary" />
    <div class="text-h5 text-primary text-center">
      No Tasks
    </div>
  </div>
  </q-page>
</template>

<script>
export default {
  data(){
    return{
      newTask : '',
      tasks : [
        // {
        //   title : 'Wake Up at 5:00 am',
        //   done : false
        // },
        // {
        //   title : 'Complete morning walk by 7:00 am',
        //   done : false
        // },
        // {
        //   title : 'Meeting at 11:00 am',
        //   done : false
        // },
        // {
        //   title : 'Lunch at 2:00 pm',
        //   done : false
        // },
        // {
        //   title : 'Scrum Call at 6:00 pm',
        //   done : false
        // },
        // {
        //   title : 'Dinner at 9:00 pm',
        //   done : false
        // }
      ]
    }
  },
  methods : {
    deleteTask(index){
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index,1)
        this.$q.notify('Task is deleted')
      })
      
    },

    addTask(){
      this.tasks.push({
        title : this.newTask,
        done : false
      })
      this.newTask = ''
    }
  }
}
</script>


<style lang="scss">
.done{
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}

.no-tasks{
  opacity: 0.5;
}
</style>