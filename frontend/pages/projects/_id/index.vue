<template>
  <v-card>
    <v-card-title> Welcome to the {{ project.name }} project </v-card-title>
    <v-card-text
      class="highlight context-markup"
      style="white-space: pre-wrap"
      v-html="guidelineHtml"
    />
  </v-card>
</template>

<script>
import { toRefs, useContext } from '@nuxtjs/composition-api'
import { marked } from 'marked'
import DOMPurify from 'dompurify'
import { useProjectItem } from '@/composables/useProjectItem'

export default {
  layout: 'project',

  middleware: ['check-auth', 'auth', 'setCurrentProject'],

  validate({ params }) {
    return /^\d+$/.test(params.id)
  },

  setup() {
    const { params } = useContext()
    const projectId = params.value.id
    const { state: projectState, getProjectById } = useProjectItem()
    getProjectById(projectId)
    return { ...toRefs(projectState), projectId }
  },
  computed: {
    guidelineHtml() {
      console.log(this.project)
      return DOMPurify.sanitize(marked.parse(this.project.guideline || ''))
    }
  },
  methods: {
    next() {
      this.e6 = Math.max(1, (this.e6 + 1) % (this.items.length + 1))
    },
    prev() {
      this.e6 = Math.max(1, this.e6 - 1)
    }
  }
}
</script>
