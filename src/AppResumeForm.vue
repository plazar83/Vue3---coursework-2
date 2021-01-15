<template>
    <form class="card card-w30" @submit.prevent="submitHandler">
        <AppSelect
            label="Тип блока"
            :options="options"
            @selected="setSelected"
            v-model.value="type"
        />
        <AppTextarea
            label="Значение"
            placeholder="Введите значение"
            @input="updateValue"
            v-model.value="value"
        />
        <button :disabled="checkLength" class="btn primary">Добавить</button>
    </form>
</template>

<script>
  import AppTextarea from './AppTextarea'
  import AppSelect from './AppSelect'
  export default {
    emits: ['formSubmit'],
    data() {
      return {
        options: {
          title: 'Заголовок',
          subtitle: 'Подзаголовок',
          avatar: 'Аватар',
          text: 'Текст'
        },
        type: 'title',
        value: '',
      }
    },
    methods: {
      submitHandler() {
        this.$emit('formSubmit', {type: this.type, value: this.value})
        this.type = 'title'
        this.value = ''
      },
      setSelected(value) {
        this.type = value
      },
      updateValue(value) {
        this.value = value
      }
    },
    computed: {
      checkLength(){
        return this.value.length < 4
      }
    },
    components: {AppSelect, AppTextarea}
  }
</script>
