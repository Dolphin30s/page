<template>
  <div
    class="bg-white hover:shadow-md shadow rounded px-3 p-2 border-l-4"
    v-bind:class="`border-blue-400`"
  >
    <div class="flex justify-between">
      <p
        class="text-gray-700 font-semibold font-sans tracking-wide text-sm mr-3"
      >
        Phone: +15145590578
      </p>
    </div>
    <div class="flex justify-between items-center">
      <div class="flex flex-wrap items-center">
        <span class="text-sm text-gray-600"> {{ task.creator }} </span>
      </div>
      <avatar
        class="border border-white"
        :name="task.creator"
        :size="6"
        :tooltip="true"
      ></avatar>
    </div>
  </div>
</template>
<script>
import Avatar from "./Avatar.vue";
export default {
  components: {
    Avatar,
  },
  props: {
    task: {
      badge: {
        color: {
          type: String,
          default: "gray",
        },
      },
      creator: Object,
      required: true,
      default: () => ({}),
    },
  },
};
</script>
