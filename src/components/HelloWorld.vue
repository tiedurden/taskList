<template>
  <div class="TaskList">
  <header>
    <label for="taskName"> Create New Task </label>
    <div class="TaskList__firstRow">
      <input class="TaskList__input" type="text" v-model="taskName">
      <button class="TaskList__button TaskList__button--simpleAdd" @click="addTask"> add </button>
    </div>
    <button class="TaskList__button" @click="toggleModal">more details</button>
  </header>
  <section>
    <h4> All Tasks: </h4>
    <ul>
      <li v-for="task in sortedTaskList">
        <TaskItem :task="task"></TaskItem>
      </li>
    </ul>
  </section>
    <TaskModal v-if="shown" >
      <label> Task name </label>
      <input class="TaskList__input" type="text" v-model="taskName" >
      <label> Location </label>
      <input  class="TaskList__input" type="text" v-model="taskLocation" placeholder="optional">
      <div>
        <label for="category" >Category</label>
        <select class="TaskList__input" v-model="taskCategory">
          <option value=""> Select </option>
          <option value="Operational"> Operational </option>
          <option value="Implementation"> Implementation </option>
          <option value="Organizational"> Organizational </option>
        </select>
        <label for="dateField" >Due date</label>
        <input
            class="TaskList__input TaskList__input--date"
            type="date"
            v-model="taskDate"
            @change="valiDate">
      </div>
      <div>
        <button class="TaskList__button" @click="addTask"> add </button>
      </div>
    </TaskModal>
  </div>
</template>

<script setup lang="ts">
import { computed, reactive, ref } from 'vue'
import TaskModal from "@/components/TaskModal.vue";
import TaskItem from "@/components/TaskItem.vue";

type Category = "Operational" | "Implementation" | "Organizational"

interface Task {
  id: number;
  name: string;
  dueDate: Date;
  location?: string;
  category: Category;
}

const taskList: Task[] = reactive([
  {
    id: 1,
    name: "task1",
    dueDate: new Date(2023, 6, 19),
    location: "Hamburg",
    category: "Operational",
  },
  {
    id: 2,
    name: "task2",
    dueDate: new Date(2023, 6, 20),
    location: "Hamburg",
    category: "Organizational",
  },
]);

const sortedTaskList = computed(() => {
  return taskList.sort(function(a,b) {
    let difference = new Date(a.dueDate) - new Date(b.dueDate)
    return difference
  });
});

const taskName = ref("");
const taskDate = ref("");
const taskLocation = ref("")
const taskCategory = ref("");
const shown = ref(false);

const clearFormFields = () => {
  taskName.value = "";
  taskDate.value = "";
}

const addTask = () => {
  taskList.push({
    id: taskList.length + 1,
    name: taskName.value == "" ? "task" + (taskList.length + 1) : taskName.value,
    dueDate: taskDate.value == "" ? new Date() : new Date(taskDate.value),
    location: taskLocation.value,
    category: isCategory(taskCategory.value) ? taskCategory.value : "Operational",
  })
  clearFormFields();
  shown.value = false
}

const toggleModal = () => {
  shown.value = !shown.value;
}
function isCategory(category: string): category is Category {
  return ["Operational", "Implementation", "Organizational"].indexOf(category) !== -1;
}

const valiDate = () => {
  let date = new Date(taskDate.value)
  if ( date <= new Date()) {
    alert("Date cannot be today or the past!");
    taskDate.value = new Date().toLocaleDateString()
  }
}
</script>

<style scoped lang="scss">
.TaskList {
  display: flex;
  flex-direction: column;

  header {
  margin-bottom: 15px;
  }

  ul {
    margin-top: 5px;
    padding: 0;
  }

  li {
    border: 1px solid black;
    border-bottom: none;
    list-style-type: none;
    border-radius: 4px;

    &:last-child{
     border-bottom: 1px solid black;;
    }
  }

  &__firstRow {
  display: flex;
  }

  &__input {
    display: block;
    height: 30px;
    width: 190px;
    border: 1px solid black;
    border-radius: 4px;
    margin: 5px 0 10px 0;
    padding-left: 10px;

    &--date {
      margin-bottom: 30px;
    }
  }

  &__button {
    margin-right: 5px;
    background-color: white;
    font-weight:600;
    border: 1px solid black;
    border-radius:5px;
    width: 52%;
    height: 30px;

    &--simpleAdd {
      margin: 5px 0 0 5px;
    }
  }
}
</style>

