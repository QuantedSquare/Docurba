<template>
  <v-container>
    <v-row justify="center">
      <v-col
        v-for="letter in alphabet"
        :key="letter"
        cols="auto"
      >
        <v-btn
          elevation="2"
          icon
          :href="`#${letter}`"
          :disabled="!groupedWords[letter]"
        >
          <b>{{ letter }}</b>
        </v-btn>
      </v-col>
    </v-row>
    <v-row v-for="(words, letter) in groupedWords" :key="letter">
      <v-col cols="12">
        <h2 :id="letter" class="text-h2">
          {{ letter }}
        </h2>
      </v-col>
      <v-col v-for="word in words" :key="word.title" cols="12" sm="6" md="4">
        <v-card :id="word.title" :href="`#${word.title}`" :ripple="false">
          <v-card-title class="break-word">
            {{ word.title }}
          </v-card-title>
          <v-col cols="12">
            <v-card-text>
              <nuxt-content :document="word" />
            </v-card-text>
          </v-col>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
<script>
import { groupBy } from 'lodash'

export default {
  data () {
    return {
      alphabet: 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split(''),
      wordsData: []
    }
  },
  async fetch () {
    this.wordsData = await this.$content('Glossaire', {
      deep: true
    }).fetch()
  },
  computed: {
    groupedWords () {
      return groupBy(this.wordsData, (word) => {
        return word.title[0]
      })
    }
  }
}
</script>
