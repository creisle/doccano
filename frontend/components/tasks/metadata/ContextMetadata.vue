<template>
  <v-card>
    <v-card-text class="highlight context-markup" style="white-space: pre-wrap" v-html="textHtml" />
  </v-card>
</template>

<script lang="ts">
import Vue from 'vue'

import { marked } from 'marked'
import DOMPurify from 'dompurify'

import '@/assets/style/context.css'

export default Vue.extend({
  props: {
    text: {
      type: String,
      default: () => '',
      required: true
    }
  },

  computed: {
    textHtml() {
      return DOMPurify.sanitize(marked.parse(this.text))
    }
  }
})
</script>
