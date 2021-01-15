<template>
    <div class="form-control">
        <label :for="ID">{{label}}</label>
        <select :id="ID" @change="changeSelect" :value="modelValue">
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
    methods: {
      changeSelect(event) {
        this.$emit('selected', event.target.value);
      }
    },
    computed: {
      ID() {
        return '_' + Math.random().toString(36).substr(2, 9)
      }
    }
  };
</script>