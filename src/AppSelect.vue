<template>
    <div class="form-control">
        <label :for="id">{{label}}</label>
        <select id="type" @change="changeSelect" :value="modelValue">
            <option v-for="(option, index) in options" :key="index" :value="index">{{option}}</option>
        </select>
    </div>
</template>

<script>
  export default {
    emits: ['selected'],
    props: {
      label: String,
      options: Object,
      modelValue: String,
    },
    data() {
      return {
        id: Math.random(),
      };
    },
    methods: {
      changeSelect(event) {
        this.$emit('selected', event.target.value);
      }
    },
  };
</script>