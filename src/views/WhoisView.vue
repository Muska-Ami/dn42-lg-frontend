<script setup lang="ts">
import { ref } from 'vue'
import Api from '@/scripts/api'
import { useRoute } from 'vue-router';
import { ElNotification } from 'element-plus';

const route = useRoute()

const value = route.params.value

let loaded = ref(false)
let detail = ref('')

    ; (async () => {
        /*@ts-ignore*/
        const req_result = await Api.whois(value)
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

<template>
    <h2>Whois information: {{ value }}</h2>
    <div style="margin-top: 1rem">
        <el-skeleton v-if="!loaded" :rows="5" animated />
        <Suspense v-else>
            <template #default>
                <pre>{{ detail }}</pre>
            </template>
        </Suspense>
    </div>
</template>