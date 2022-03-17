<template>
  <div>
    <div class="flex bg-gray-100 flex-wrap p-4 pl-10">
      <h3 class="text-3xl text-gray-800 font-bold py-1 pr-8">
        {{ kanbanTitle }}
        <a
          href="#"
          class="px-2 text-gray-400 hover:text-gray-600 transition duration-300 ease-in-out focus:outline-none"
          ><i class="fas fa-edit"></i>
        </a>
      </h3>

      <div class="flex items-center py-1 pr-8">
        <p class="px-2 text-gray-500">{{ kanbanMembers.length }} members</p>

        <template v-for="(member, memberIndex) in kanbanMembers">
          <template v-if="memberIndex < 5">
            <avatar
              :key="memberIndex"
              class="cursor-pointer transform hover:-translate-y-1 transition duration-300"
              :class="{ '-ml-3': memberIndex > 0 }"
              :name="member.name"
              :size="10"
              :borderSize="2"
              :tooltip="true"
            ></avatar>
          </template>
        </template>

        <span
          v-if="kanbanMembers.length > 5"
          class="z-10 flex items-center justify-center font-semibold text-gray-800 text-sm w-10 h-10 rounded-full bg-gray-300 border-2 border-white -ml-3 pr-2"
          >+{{ kanbanMembers.length - 5 }}</span
        >
        <button
          @click="createMember()"
          class="z-20 items-center justify-center w-10 h-10 mr-2 text-indigo-100 transition-colors duration-150 bg-indigo-700 rounded-full focus:outline-none hover:bg-indigo-800 border-2 border-white -ml-3"
        >
          <i class="fas fa-plus"></i>
        </button>
      </div>
    </div>
    <hr />
  </div>
</template>
<script>
import Avatar from "./Avatar.vue";
export default {
  inject: ["eventHub"],
  components: {
    Avatar,
  },
  props: {
    kanbanTitle: {
      type: String,
      default: "no title",
    },
    kanbanID: {
      type: Number,
      default: null,
    },
    kanbanMembers: {
      type: Array,
      default: () => [],
    },
  },
  methods: {
    createMember() {
      console.log("add member from kanbanbar");
      this.eventHub.$emit("add-member");
    },
  },
};
</script>

<style scoped>
.rotate-45 {
  --transform-rotate: 45deg;
  transform: rotate(45deg);
}

.group:hover .group-hover\:flex {
  display: flex;
}
</style>