
<template>
  <div id="app" class="flex">
    <div class="flex min-h-screen">
      <side-bar></side-bar>
    </div>
    <div
      class="relative min-w-screen border-r flex-grow overflow-x-auto"
      v-bind:class="{ 'overflow-hidden': kanbanIsLoading }"
    >
      <kanban-bar
        :kanbanTitle="kanban.title"
        :kanbanId="kanban.id"
        :kanbanMembers="kanban.members"
      ></kanban-bar>
      <transition
        enter-active-class="transition duration-500 ease-out transform"
        enter-class=" opacity-0 bg-blue-200"
        leave-active-class="transition duration-300 ease-in transform"
        leave-to-class="opacity-0 bg-blue-200"
      >
        <div
          v-if="kanbanIsLoading"
          class="z-50 overflow-auto absolute inset-0 bg-gray-400 bg-opacity-75 flex"
        >
          <loading-animation :size="100"></loading-animation>
        </div>
      </transition>
      <kanban :kanbanData="kanban"></kanban>
    </div>
  </div>
</template>

<script>
import LoadingAnimation from "./components/LoadingAnimation.vue";
import Kanban from "./components/Kanban.vue";
import KanbanBar from "./components/KanbanBar.vue";
import SideBar from "./components/SideBar.vue";
import Vue from "vue";

export default {
  name: "App",
  components: {
    LoadingAnimation,
    Kanban,
    KanbanBar,
    SideBar,
  },
  data() {
    return {
      eventHub: new Vue(),
      kanbanIsLoading: false,
      kanban: {},
    };
  },
  provide() {
    return {
      eventHub: this.eventHub,
    };
  },
  created() {
    this.eventHub.$on("switch-kanban", (kanbanID) => {
      this.switchKanban(kanbanID);
    });
  },
  mounted() {
    this.switchKanban(2);
  },
  methods: {
    switchKanban(kanbanID) {
      this.kanbanIsLoading = true;

      let kanban = {};

      // MOCK KANBAN DATA START --------------------------------------------
      if (kanbanID === 1) {
        kanban = {
          id: 1,
          title: "A different Kanban",
          members: [
            {
              id: 7,
              name: "Siamak Samie",
              role: "admin",
            },
            {
              id: 8,
              name: "Yuna Guivarch",
              role: "user",
            },
          ],
          badges: [
            {
              id: 4,
              title: "Design",
              color: "purple",
            },
            {
              id: 5,
              title: "Backend",
              color: "indigo",
            },
            {
              id: 6,
              title: "QA",
              color: "blue",
            },
            {
              id: 7,
              title: "Frontend",
              color: "green",
            },
            {
              id: 80,
              title: "Test",
              color: "yellow",
            },
            {
              id: 9,
              title: "Bugs",
              color: "red",
            },
          ],
          priorities: [
            {
              title: "High",
              icon: "fa-arrow-up",
              color: "red",
            },
            {
              title: "Medium",
              icon: "fa-arrow-up",
              color: "yellow",
            },
            {
              title: "Low",
              icon: "fa-arrow-down",
              color: "green",
            },
            {
              title: "Not Set",
              icon: "fa-circle",
              color: "gray",
            },
          ],
          archive: [],
          rows: [
            {
              id: "3",
              title: "new kanban",
              columns: [
                {
                  title: "new kanban row",
                  tasks: [
                    {
                      id: 100,
                      title: "new Kanban ",
                      creator: "Siamak Samie",
                      assignedTo: [
                        {
                          id: 7,
                          name: "Siamak Samie",
                          role: "admin",
                        },
                        {
                          id: 8,
                          name: "Yuna Guivarch",
                          role: "user",
                        },
                      ],
                      date: "Sep 14",
                      badge: {
                        id: 9,
                        title: "Bugs",
                        color: "red",
                      },
                      priority: {
                        title: "Medium",
                        icon: "fa-arrow-up",
                        color: "yellow",
                      },
                    },
                  ],
                },
              ],
            },
          ],
        };
      } else if (kanbanID === 2) {
        kanban = {
          id: 2,
          title: "Dispatcher Toronto",
          members: [
            {
              id: 1,
              name: "Nico Brodeur",
              role: "admin",
            },
            {
              id: 2,
              name: "Siamak Samie",
              role: "user",
            },
            {
              id: 3,
              name: "Yuna guivarch",
              role: "user",
            },
            {
              id: 4,
              name: "Gurpreet Sandhu",
              role: "user",
            },
            {
              id: 5,
              name: "bob vincentti",
              role: "user",
            },
            {
              id: 6,
              name: "hello world",
              role: "user",
            },
          ],

          rows: [
            {
              id: "1",
              title: "Dev Team",
              columns: [
                {
                  id: 2,
                  title: "Backlog",
                  tasks: [
                    {
                      id: 1,
                      name: "Siamak Samie",
                      phone: "15145590578",
                    },
                    {
                      id: 1,
                      name: "Siamak Samie",
                      phone: "15145590578",
                    },
                  ],
                },
                {
                  title: "In Progress",
                  tasks: [
                    {
                      id: 1,
                      name: "Siamak Samie",
                      phone: "15145590578",
                    },
                  ],
                },
                {
                  title: "Review",
                  tasks: [
                    {
                      id: 1,
                      name: "Siamak Samie",
                      phone: "15145590578",
                    },
                  ],
                },
                {
                  title: "Done",
                  tasks: [
                    {
                      id: 1,
                      name: "Siamak Samie",
                      phone: "15145590578",
                    },
                  ],
                },
              ],
            },
            {
              id: "2",
              title: "Sales Team",
              columns: [
                {
                  id: 2,
                  title: "Backlog",
                  tasks: [
                    {
                      id: 21,
                      title: "Add discount code to checkout page",
                      creator: "melanie porque",
                      assignedTo: [
                        {
                          id: 1,
                          name: "Nico Brodeur",
                          role: "admin",
                        },
                        {
                          id: 2,
                          name: "Siamak Samie",
                          role: "user",
                        },
                      ],
                      date: "Sep 14",
                      badge: {
                        id: 9,
                        title: "Marketting",
                        color: "blue",
                      },
                      priority: {
                        title: "Medium",
                        icon: "fa-arrow-up",
                        color: "yellow",
                      },
                    },
                    {
                      id: 22,
                      title: "Provide documentation on integrations",
                      creator: "shubert  cube",
                      assignedTo: [
                        {
                          id: 1,
                          name: "Nico Brodeur",
                          role: "admin",
                        },
                        {
                          id: 2,
                          name: "Siamak Samie",
                          role: "user",
                        },
                      ],
                      badge: {
                        id: 9,
                        title: "Marketting",
                        color: "blue",
                      },
                      date: "Sep 12",
                      priority: {
                        title: "Medium",
                        icon: "fa-arrow-up",
                        color: "yellow",
                      },
                    },
                  ],
                },
                {
                  title: "In Progress",
                  tasks: [],
                },

                {
                  title: "Done",
                  tasks: [
                    {
                      id: 214,
                      title: "Add discount code to checkout page",
                      creator: "faraway lawson",
                      assignedTo: [
                        {
                          id: 1,
                          name: "Nico Brodeur",
                          role: "admin",
                        },
                        {
                          id: 2,
                          name: "Siamak Samie",
                          role: "user",
                        },
                      ],
                      date: "Sep 14",
                      badge: {
                        id: 9,
                        title: "Marketting",
                        color: "blue",
                      },
                      priority: {
                        title: "High",
                        icon: "fa-arrow-up",
                        color: "red",
                      },
                    },
                  ],
                },
              ],
            },
          ],
        };
      }

      // MOCK KANBAN DATA END --------------------------------------

      setTimeout(() => {
        this.kanban = kanban;
        this.kanbanIsLoading = false;
      }, 2000);
    },
  },
};
</script>
