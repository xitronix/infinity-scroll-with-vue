<template>
  <q-page class="flex flex-center">
    <div id="q-app" style="min-height: 100vh;">
      <div class="q-pa-md">
        <q-infinite-scroll @load="onLoad" :offset="250" class="q-gutter-md">
          <q-img v-for="(item, index) in items" :key="index" :src="item.images.downsized.url" spinner-color="white"
            style="height: 140px; max-width: 150px" />
          <template v-slot:loading>
            <div class="row justify-center q-my-md">
              <q-spinner-dots color="primary" size="40px"></q-spinner-dots>
            </div>
          </template>
        </q-infinite-scroll>
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  setup: function () {
    const items = ref([])

    return {
      items,
      async onLoad(index, done) {
        const GIFS_COUNT = 49;
        const response = await fetch(`https://api.giphy.com/v1/gifs/trending?api_key=OO8Yf9k8x85cJjzGekLZ1ftrZ64cXIZy&offset=${index*GIFS_COUNT}&limit=${GIFS_COUNT}`)
        const { pagination, data, meta } = await response.json()
        console.log(pagination, data.images?.lenght, meta)
        items.value.push(...data)
        done()
      }
    }
  },
})
</script>
