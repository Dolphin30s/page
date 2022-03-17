<template>
  <div>
    <transition
      enter-active-class="transition duration-500 ease-out transform"
      enter-class=" opacity-0 bg-blue-200"
      leave-active-class="transition duration-300 ease-in transform"
      leave-to-class="opacity-0 bg-blue-200"
    >
      <div
        v-if="modalOpen"
        class="overflow-auto fixed inset-0 bg-gray-700 bg-opacity-50 z-30"
      ></div>
    </transition>

    <transition
      enter-active-class="transition duration-300 ease-out transform "
      enter-class="scale-95 opacity-0 -translate-y-10"
      enter-to-class="scale-100 opacity-100"
      leave-active-class="transition duration-150 ease-in transform"
      leave-class="scale-100 opacity-100"
      leave-to-class="scale-95 opacity-0"
    >
      <!-- Modal container -->

      <div
        v-if="modalOpen"
        class="fixed inset-0 z-40 flex items-start justify-center"
      >
        <!-- Close when clicked outside -->
        <div
          class="overflow-auto fixed h-full w-full"
          @click="modalOpen = false"
        ></div>
        <div
          style="width: 700px; min-height: 300px; max-height: 80%"
          class="flex flex-col overflow-auto z-50 w-100 bg-white rounded-md shadow-2xl m-10"
        >
          <!-- Task heading -->
          <div class="flex justify-between p-5 bg-indigo-800 border-b">
            <div class="space-y-1">
              <h1 class="text-2xl text-white pb-2">Create new task</h1>
              <p class="text-sm font-medium leading-5 text-gray-500">
                Adding new member to this Kanban
              </p>
            </div>
            <div>
              <button
                type="button"
                class="focus:outline-none flex flex-col items-center text-gray-400 hover:text-gray-500 transition duration-150 ease-in-out pl-8"
                @click="modalOpen = false"
              >
                <i class="fas fa-times"></i>
                <span
                  class="text-xs font-semibold text-center leading-3 uppercase"
                >
                  Esc
                </span>
              </button>
            </div>
          </div>
          <!-- Task container -->
          <form class="space-y-6 overflow-auto px-8 py-6">
            <!-- Flow options -->
            <div class="space-y-6">
              <div class="space-y-4">
                <span
                  class="block text-xs font-bold leading-4 tracking-wide uppercase text-gray-600"
                >
                  Assign To
                </span>
                <vSelect
                  multiple
                  :options="allUsers"
                  v-model="selectedMembers"
                  label="name"
                  placeholder="Select members"
                  style="margin-top: 7px"
                  class="text-gray-400"
                >
                  <template slot="option" slot-scope="option">
                    <avatar
                      class="mr-3 m-1 float-left"
                      :name="option.name"
                      :size="4"
                    ></avatar>
                    <p class="inline">{{ option.name }}</p>
                  </template>
                  <template #no-options="{ search, searching, loading }">
                    No result .
                  </template>
                </vSelect>
              </div>
            </div>
            <div class="w-full grid sm:grid-cols-2 gap-3 sm:gap-3">
              <button
                type="button"
                class="px-4 py-3 border border-gray-200 rounded text-gray-600 hover:bg-gray-50 hover:border-gray-300 hover:text-gray-600 transition duration-300 ease-in-out"
                @click="modalOpen = false"
              >
                Cancel
              </button>
              <button
                type="button"
                class="px-4 py-3 border border-transparent rounded text-white bg-indigo-600 hover:bg-indigo-500 transition duration-300 ease-in-out"
                @click="saveMember($event)"
              >
                Add Members
              </button>
            </div>
            <figure class="bg-gray-100 rounded-xl h-64 overflow-y-auto">
              <div class="pt-6 md:p-8 text-center md:text-left space-y-4">
                <figcaption class="font-medium">
                  <p class="pb-5 text-gray-500">
                    There are a total of {{ kanbanData.members.length }} members
                    in the kanban
                  </p>

                  <label class="block pb-5">
                    <input
                      type="text"
                      v-model="filter"
                      placeholder="Filter members"
                      class="px-3 py-3 placeholder-gray-400 text-gray-700 rounded border border-gray-400 w-full pr-10 outline-none text-md leading-4"
                    />
                  </label>

                  <template v-for="(member, memberIndex) in filtered">
                    <template v-if="memberIndex < 5">
                      <div
                        class="flex justify-between items-center border-b p-1"
                        :key="memberIndex"
                      >
                        <div class="flex items-center">
                          <avatar
                            class="mr-3"
                            :name="member.name"
                            :size="6"
                          ></avatar>
                          <span class="py-2 mr-3 text-gray-600">{{
                            member.name
                          }}</span>
                        </div>
                        <a
                          class="cursor-pointer text-gray-400 text-sm hover:text-gray-600 transition duration-300 ease-in-out"
                          >remove</a
                        >
                      </div>
                    </template>
                  </template>
                </figcaption>
              </div>
            </figure>
          </form>
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
import vSelect from "vue-select";
import Avatar from "./Avatar.vue";

export default {
  inject: ["eventHub"],
  components: {
    vSelect,
    Avatar,
  },

  props: {
    kanbanData: Object,
  },
  data() {
    return {
      filter: "",
      modalOpen: false,
      isSavingMember: false,
      kanbanID: null,
      selectedMembers: null,
      allUsers: [
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
    };
  },
  created() {
    this.eventHub.$on("add-member", () => {
      this.modalOpen = true;
    });
  },
  computed: {
    filtered() {
      const regex = new RegExp(this.filter, "i");
      return this.kanbanData.members.filter((e) => {
        return !this.filter || e.name.match(regex);
      });
    },
  },
  methods: {
    saveMember(event) {
      event.target.disabled = true;
      this.eventHub.$emit("save-members", this.selectedMembers);
      this.modalOpen = false;
      this.selectedMembers = null;
    },
  },
};
</script>
