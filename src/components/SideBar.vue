<template>
  <div class="w-64 px-8 py-4 bg-gray-100 border-r overflow-auto">
    <div class="text-center">
      <i class="fas fa-th fa-3x p-3 text-indigo-800"></i>
      <h1 class="text-3xl tracking-wide text-indigo-800 font-bold my-0">
        KANYEBAN
      </h1>
      <p class="text-xs text-indigo-800 -mt-2">
        The greatest kanban of all time
      </p>
    </div>

    <nav class="mt-8">
      <h3
        class="text-sm font-semibold text-gray-600 uppercase tracking-wide text-left"
      >
        General
      </h3>
      <div class="my-2 -mx-3 pb-2">
        <a
          href="#"
          class="flex justify-between items-center px-3 py-2 hover:bg-gray-200 rounded-lg"
        >
          <span class="text-sm font-medium text-gray-900">Dashboard</span>
        </a>
      </div>
      <hr />
      <h3
        class="mt-8 text-sm font-semibold text-gray-600 uppercase tracking-wide text-left"
      >
        Kanbans
      </h3>
      <div class="my-2 -mx-3 pb-2">
        <template v-for="(kanban, kanbanIndex) in kanbanList">
          <a
            :key="kanbanIndex"
            href="#kanban"
            @click="switchKanban(kanban.id)"
            class="flex justify-between items-center px-3 py-2 hover:bg-gray-200 rounded-lg"
          >
            <span class="text-sm font-medium text-gray-700">{{
              kanban.name
            }}</span>
          </a>
        </template>
      </div>

      <button
        class="mt-4 -ml-1 flex items-center text-sm font-medium text-gray-600 hover:text-gray-800 transition duration-300 ease-in-out focus:outline-none"
      >
        <i class="fas fa-plus"></i>
        <span class="ml-1 font-bold">New Project</span>
      </button>
    </nav>
  </div>
</template>
<script>
export default {
  inject: ["eventHub"],

  data() {
    return {
      kanbanList: {},
    };
  },
  mounted() {
    this.getKanbans();
  },

  methods: {
    switchKanban(kanbanID) {
      this.eventHub.$emit("switch-kanban", kanbanID);
    },
    getKanbans() {
      let kanbanList = [
        {
          id: 1,
          name: "New Feature",
        },
        {
          id: 2,
          name: "Sales",
        },
      ];

      this.kanbanList = kanbanList;
    },
  },
};
</script>
