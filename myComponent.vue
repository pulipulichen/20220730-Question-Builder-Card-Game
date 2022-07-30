<script setup>
import { ref, reactive, onMounted } from 'vue'

const ary1 = reactive([])

async function fetchList(url) {
  let response = await fetch(url)
  let text = await response.text()
  return text.trim().split('\n')
}

//onMounted(() => window.addEventListener('mousemove', update))
//onUnmounted(() => window.removeEventListener('mousemove', update))
onMounted(async () => {
  ary1.push(...(await fetchList('主詞.txt')))

  // ary1.push('aaas')

  //console.log(ary1Response)
  //let ary1 = ary1Response.text().split()
  // console.log(ary1)
})
</script>

<template>
{{ ary1 }}
</template>