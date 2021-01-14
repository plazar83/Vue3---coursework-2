<template>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addBlock">
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
    <AppResumeBlocks :blocks="blocks"/>
  </div>
  <div class="container">
    <p>
      <AppButton btnColor="primary" @action="loadComments">Загрузить комментарии</AppButton>
    </p>
    <AppLoader v-if="loading"></AppLoader>
    <AppComments v-else @action="loadComments" :loading="loading" :comments="comments" />
  </div>
</template>

<script>
import AppResumeBlocks from './AppResumeBlocks'
import AppComments from './AppComments'
import AppTextarea from './AppTextarea'
import AppSelect from './AppSelect'
import AppButton from './AppButton'
import AppLoader from './AppLoader'
export default {
  // https://vue-coursework2-default-rtdb.firebaseio.com/
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
      blocks: [],
      comments: null,
      loading: false
    }
  },
  methods: {
    addBlock() {
      this.blocks.push({type: this.type, value: this.value})
      this.type = 'title'
      this.value = ''
    },
    async loadComments() {
      this.loading = true
      try {
        const response = await fetch('https://jsonplaceholder.typicode.com/comments?_limit=42')
        const data = await response.json()
        this.comments = Object.keys(data).map(key => {
          return { ...data[key], id: key }
        })
      } catch (e) {
        console.log(e)
      }
      this.loading = false
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
      return this.value.length < 3
    }
  },
  components: {AppResumeBlocks, AppComments, AppSelect, AppTextarea, AppButton, AppLoader}
}
</script>

<style>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }
</style>
