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
    <div class="card card-w70">
      <AppResumeBlocks :blocks="blocks"/>
      <AppButton v-show="blocks.length !== 0" btnColor="primary" :disabled="saving" @action="saveBlocks">Сохранить резюме</AppButton>
    </div>
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
      loading: false,
      saving: false,
      resumeId: 0
    }
  },
  mounted() {
    this.loadBlock()
    this.resumeId = this.ID()
  },
  methods: {
    addBlock() {
      // this.blocks.push({type: this.type, value: this.value})
      this.blocks.push({resumeId: this.resumeId, type: this.type, value: this.value})
      console.log(this.blocks)
      this.type = 'title'
      this.value = ''
    },
    async saveBlocks() {
      this.saving = true
      const firebase = await fetch('https://vue-coursework2-default-rtdb.firebaseio.com/blocks.json', {
        method: "post",
        headers: {
          'Content-type': 'application/json'
        },
        body: JSON.stringify(this.blocks)
      })
      this.saving = false
      console.log(firebase)
    },
    async loadBlock(){
      try {
        const response = await fetch('https://vue-coursework2-default-rtdb.firebaseio.com/blocks.json')
        const data = await response.json()
        const blocks = Object.keys(data).map(key => {
          return data[key]
        })
        this.blocks = blocks[0]
      } catch (e) {
        console.log(e)
      }
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
    },
    ID(){
      return '_' + Math.random().toString(36).substr(2, 9)
    }
  },
  computed: {
    checkLength(){
      return this.value.length < 4
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
