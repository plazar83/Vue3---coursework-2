<template>
    <div class="card card-w70">
        <div v-if="blocks.length !== 0">
            <component v-for="(block, index) in blocks" :key="index" :is="componentName(block.type)" v-bind="{value: block.value}"></component>
        </div>
        <h3 v-else>Добавьте первый блок, чтобы увидеть результат</h3>
    </div>
</template>

<script>
  import AppResumeTitle from './AppResumeTitle'
  import AppResumeSubtitle from './AppResumeSubtitle'
  import AppResumeAvatar from './AppResumeAvatar'
  import AppResumeText from './AppResumeText'

  export default {
    props: ['blocks'],
    methods: {
      componentName(blockType) {
        return 'app-resume-' + blockType
      }
    },
    components: {AppResumeTitle, AppResumeSubtitle, AppResumeAvatar, AppResumeText}
  }
</script>
