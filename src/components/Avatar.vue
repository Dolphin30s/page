<template>
  <!-- Component Start -->
  <div
    class="rounded-full inline relative flex flex-col items-center group"
    :class="`w-${size} h-${size}`"
  >
    <div
      v-if="tooltip"
      class="absolute top-0 flex flex-col items-center hidden mb-6 group-hover:flex"
      :class="`mt-${size}`"
    >
      <div class="w-3 h-3 -mb-2 rotate-45 bg-gray-800"></div>
      <span
        class="relative z-10 p-2 text-xs leading-none text-white whitespace-no-wrap bg-gray-800 rounded shadow-lg"
        >{{ name }}</span
      >
    </div>
    <img
      class="rounded-full"
      :class="`border-${borderSize} border-${borderColor} ${
        borderSize === 0 ? 'border' : ''
      }`"
      :src="avatarUrl(name)"
      alt="Avatar"
    />
  </div>

  <!-- Component End  -->
</template>
<script>
export default {
  props: {
    name: {
      type: String,
      default: "john Doe",
    },
    size: {
      type: Number,
      default: 10,
    },

    tooltip: {
      type: Boolean,
      default: false,
    },

    borderSize: {
      type: Number,
      default: 0,
    },

    borderColor: {
      type: String,
      default: "white",
    },
  },
  methods: {
    avatarUrl(name) {
      var hash = 0;
      for (var i = 0; i < name.length; i++) {
        hash = name.charCodeAt(i) + ((hash << 5) - hash);
      }
      var c = (hash & 0x00ffffff).toString(16).toUpperCase();
      var color = "00000".substring(0, 6 - c.length) + c;
      return (
        "https://ui-avatars.com/api/?name=" +
        name +
        "&background=" +
        color +
        "&color=fff"
      );
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
