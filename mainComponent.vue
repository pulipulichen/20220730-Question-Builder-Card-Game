<script setup>
import { ref, reactive, onMounted } from 'vue'

const types = reactive({
  'subjects': [],
  'adj': [],
  'objects': []
})

async function fetchList(url) {
  let response = await fetch(url)
  let text = await response.text()
  return text.trim().split('\n')
}

//onMounted(() => window.addEventListener('mousemove', update))
//onUnmounted(() => window.removeEventListener('mousemove', update))
onMounted(async () => {
  for (let type in types) {
    types[type].push(...(await fetchList(`./data/${type}.txt`)))
  }
})
</script>

<template>
<template v-for="(type, key) in types" class="type" :class="key">
<div v-for="term in type" class="card">

  <div class="header description">

    <div class="block header">
      請說出
    </div>

    <div class="block" :class="{'current-type' : (key === 'subjects')}">
      1. 主詞
    </div>

    <div class="block" :class="{'current-type' : (key === 'adj')}">
      2. 形容詞
    </div>

    <div class="block" :class="{'current-type' : (key === 'objects')}">
      3. 受詞
    </div>

    <div class="block footer">
      是什麼？
    </div>

  </div>

  <div v-if="key === 'subjects'" class="numberCircle">1</div>
  <div v-if="key === 'adj'" class="numberCircle">2</div>
  <div v-if="key === 'objects'" class="numberCircle">3</div>
  <div class="term">
    
    {{ term }}
  </div>
  
</div>
</template>
</template>

<style scoped>
/* PTCG，是一種交換式卡片遊戲，卡牌大小為63x88mm（2.5x3.5英吋） */
.card {
  border: 1px solid #CCC;
  width: 63mm;
  height: 88mm;
  display: inline-block;
  margin: 5px;

  font-size: 4mm;
  text-align: center;
}

.card .term {
  font-size: 10mm;
}

.header.description {
  margin: 0 auto;
  width: fit;
  text-align: center;
  margin-top: 10mm;
}

.header.description .block {
  display: inline-block;

  border: 1px solid #666;
  margin: 5px;
  padding: 0 3px;
}

.header.description .block.current-type {
  background-color: #666;
  color: #FFF;
}

.header.description .block.footer,
.header.description .block.header {
  border-width: 0;
  display: block;
}

.numberCircle {
  border-radius: 50%;
  width: 27px;
  height: 27px;
  padding: 6px;

  background: #fff;
  border: 2px solid #666;
  color: #666;
  text-align: center;

  font: 24px Arial, sans-serif;

  margin: 0 auto;
  margin-top: 15px;
  margin-bottom: 10px;
}

</style>