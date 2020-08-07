<template>
  <q-page class="bg-grey-3 column">
    <div class="div row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        square
        filled
        bg-color="white"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add"></q-btn>
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="index"
        clickable
        v-ripple
        @click="task.done =! task.done"
        :class="{ 'done bg-blue-1' : task.done}"
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" class="no-pointer-events"></q-checkbox>
        </q-item-section>
        <q-item-section>
          <q-item-label>{{task.name}}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn @click.stop="deleteTask(index)" flat round dense color="primary" icon="delete"></q-btn>
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary"/>
      <div class="text-h5 text-primary text-center">No tasks</div>
    </div>
  </q-page>
</template>

<script>
export default {
  name: "todo",
  data() {
    return {
      newTask: "",
      tasks: [
        {
          id: 1,
          name: "Eat",
          done: false
        },
        {
          id: 2,
          name: "Sleep",
          done: false
        }
      ]
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          dark: true,
          title: "Confirm",
          message: "Would you like to delete the task?",
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify({
            message: "Task deleted",
            icon: "announcement"
          });
        });
    },
    addTask() {
      this.tasks.push({
        id: this.tasks.length + 1,
        name: this.newTask,
        done: false
      });
      this.newTask = "";
      console.log(this.tasks);
    }
  }
};
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
