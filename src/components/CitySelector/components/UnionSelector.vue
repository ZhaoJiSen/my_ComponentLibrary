<template>
  <select class="city-selector" v-if="isShow" @change="handleChange">
    <option value="">
      {{ defaultOption }}
    </option>
    <option v-for="(info, name) in data" :value="`${ info[codeKey] }:${ name }`">
      {{ name }}
    </option>
  </select>
</template>
<script setup>
const props = defineProps({
  isShow: {
    type: Boolean,
    default() {
      return false;
    },
  },
  defaultOption: {
    type: String,
    default() {
      return "Default Option"
    }
  },
  codeKey: {
    type: String,
  },
  data: {
    type: Object,
    default() {
      return {}
    }
  }
})
const emit = defineEmits(["handleChange"])

const handleChange = (e) => {
  emit("handleChange", e.target.value);
}
</script>

<style scoped>
.city-selector {
  outline: none;
  border: 1px solid #ddd;
  border-radius: 15px;
  padding: 15px;
  margin-left: 50px;
  margin-top: 50px;
}
</style>