<template>
  <h2>Route Details</h2>
  <el-text>Server: {{ server }}</el-text>
  <div style="margin-top: 1rem">
    <el-skeleton v-if="!loaded" :rows="5" animated />
    <Suspense v-else>
      <template #default>
        <pre>{{ detail }}</pre>
      </template>
    </Suspense>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRoute } from 'vue-router'
import Api from '@/scripts/api'
import { ElNotification } from 'element-plus';

const route = useRoute()

const server = route.params.server

let loaded = ref(false)
let command = 'show route'
let detail = ref('')

  //   if (protocol == null) {
  //     command += 'all'
  //   } else {
  //     command += `all ${protocol}`
  //   }

  ; (async () => {
    /*@ts-ignore*/
    const req_result = await Api.executeBird([server], command)
    // console.log(req_result)
    detail.value = req_result.data.result[0].data
    loaded.value = true
  })().catch((e) => {
    ElNotification({
      title: "Failed to fetch",
      message: e!.toString(),
      type: "error"
    })
    detail.value = `Unable to fetch information:\n${e}`
    loaded.value = true
  })
</script>
