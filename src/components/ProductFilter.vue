<template>
  <q-card>
    <div class="q-pa-md">
      <div class="q-gutter-y-md">
        Product Filter
      </div>
    </div>

  </q-card>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'ProductFilter',
  props: ['filter',],

  data: function () {
    return {
    }
  },
  methods: {

  },
  created: async function () {

  }
})
</script>
