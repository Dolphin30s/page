<template>
  <div>
    <div
      v-for="(row, rowIndex) in kanban.rows"
      :key="row.title"
      class="mx-5 my-3"
    >
      <div class="border bg-gray-700 pl-3 pr-3 rounded py-2">
        <h2 class="text-gray-100 font-medium tracking-wide">
          {{ row.title }}
          <a
            href="#"
            class="px-2 text-gray-500 hover:text-gray-400 transition duration-300 ease-in-out focus:outline-none"
            ><i class="fas fa-edit"></i
          ></a>
        </h2>
      </div>
      <div class="flex flex-wrap">
        <div class="flex flex-1 pt-3 overflow-x-auto overflow-y-hidden">
          <div
            v-for="(column, columnIndex) in row.columns"
            :key="column.title"
            class="flex-1 bg-gray-200 px-3 py-3 column-width rounded mr-4"
          >
            <div class="flex">
              <p
                class="flex-auto text-gray-700 font-semibold font-sans tracking-wide pt-1"
              >
                {{ column.title }}
                <a
                  href="#"
                  class="px-2 text-gray-400 hover:text-gray-600 transition duration-300 ease-in-out focus:outline-none"
                  ><i class="fas fa-edit"></i>
                </a>
              </p>

              <button
                class="p-0 w-6 h-6 bg-blue-200 rounded-full hover:bg-blue-300 active:shadow-lg mouse shadow transition ease-in duration-200 focus:outline-none"
                @click="createTask(rowIndex, columnIndex)"
              >
                <i class="fas fa-plus text-white"></i>
              </button>
            </div>
            <draggable
              @start="drag = true"
              @end="drag = false"
              @update="onUpdate"
              :list="column.tasks"
              :animation="200"
              ghost-class="ghost-card"
              group="tasks"
              class="h-full list-group"
              @change="getChangeData($event, columnIndex, rowIndex)"
            >
              <task-card
                v-for="task in column.tasks"
                :key="task.id"
                :task="task"
                class="mt-3 cursor-move"
                v-on:click.native="updateTask(task.id)"
              ></task-card>
            </draggable>
          </div>
        </div>
      </div>
    </div>
    <hr class="mt-5" />

    <add-task-modal :kanbanData="kanbanData"></add-task-modal>
    <add-member-modal :kanbanData="kanbanData"></add-member-modal>
  </div>
</template>

<script>
import draggable from "vuedraggable";
import TaskCard from "./TaskCard.vue";
import AddTaskModal from "./AddTaskModal.vue";
import AddMemberModal from "./AddMemberModal.vue";

export default {
  inject: ["eventHub"],
  components: {
    TaskCard,
    draggable,
    AddTaskModal,
    AddMemberModal,
  },
  props: {
    kanbanData: {
      value: Object,
      default: null,
    },
  },

  watch: {
    kanbanData: function (newKanbanData, oldValue) {
      this.kanban = newKanbanData;
    },
  },

  created() {
    this.eventHub.$on("save-task", (taskData) => {
      this.saveTask(taskData);
    });
    this.eventHub.$on("save-members", (selectedMembers) => {
      this.saveMember(selectedMembers);
    });
  },
  methods: {
    createTask(rowIndex, columnIndex) {
      var rowName = this.kanban.rows[rowIndex].title;
      var columnName = this.kanban.rows[rowIndex].columns[columnIndex].title;
      this.eventHub.$emit("create-task", {
        rowIndex,
        rowName,
        columnIndex,
        columnName,
      });
    },

    getChangeData(event, columnIndex, rowIndex) {
      var eventName = Object.keys(event)[0];

      switch (eventName) {
        case "moved":
          this.moveEvent(event, columnIndex, rowIndex);
          break;
        case "added":
          this.addEvent(event, columnIndex, rowIndex);
          break;
        case "removed":
          this.removeEvent(event, columnIndex, rowIndex);
          break;
        default:
          alert('event "' + eventName + '" not handled: ');
      }
    },

    moveEvent(event, columnIndex, rowIndex) {
      console.log(
        "element id " +
          event.moved.element.id +
          " from table '" +
          this.kanban.rows[rowIndex].columns[columnIndex].title +
          " in row '" +
          this.kanban.rows[rowIndex].title +
          "' MOVED from index " +
          event.moved.oldIndex +
          " to index " +
          event.moved.newIndex
      );
    },
    addEvent(event, columnIndex, rowIndex) {
      console.log(
        "element id " +
          event.added.element.id +
          " was ADDED to table '" +
          this.kanban.rows[rowIndex].columns[columnIndex].title +
          "' at index " +
          event.added.newIndex
      );
    },
    removeEvent(event, columnIndex, rowIndex) {
      console.log(
        "element id " +
          event.removed.element.id +
          " was REMOVED from table '" +
          this.kanban.rows[rowIndex].columns[columnIndex].title
      );
    },
    saveMember(selectedMembers) {
      this.kanban.members = [...this.kanban.members, ...selectedMembers];
    },

    saveTask(taskData) {
      this.kanban.rows[taskData.selectedRowIndex].columns[
        taskData.selectedColumnIndex
      ].tasks.push({
        id: 1,
        title: taskData.taskName,
        description: taskData.description,
        creator: "Siamak Samie",
        assignedTo: taskData.assignedTo,
        date: "Jan 29, 2020",
        badge: taskData.badge,
        priority: taskData.priority,
      });
    },
  },

  data() {
    return {
      kanbanIsLoading: false,
      kanban: this.kanbanData,
    };
  },
};
</script>

<style scoped>
.column-width {
  min-width: 280px;
}

.ghost-card {
  opacity: 0.5;
  background: #F7FAFC;
  border: 1px solid #4299e1;
}
</style>
