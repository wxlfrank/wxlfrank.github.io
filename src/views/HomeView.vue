<script setup lang="ts">
import { onMounted, ref, watch } from 'vue'

const content = ref(
  '<nav>  \n<a>Home</a>   \n<a>About</a>  \n<a>Contact</a>   \n<div style="display: none">Pricing</div> </nav>',
)
const innerText = ref<HTMLParagraphElement | null>(null)
const textContent = ref<HTMLParagraphElement | null>(null)
let pInnerHTML: HTMLElement | null = null
let pInnerText: HTMLElement | null = null
let pTextContent: HTMLElement | null = null
function changeText(v: Event) {
  const str = (v.target as HTMLTextAreaElement).value
  if (pInnerHTML) pInnerHTML.innerHTML = str
  if (pInnerText) pInnerText.innerText = str
  if (pTextContent) pTextContent.textContent = str
}
onMounted(() => {
  watch(
    content,
    (newValue: string) => {
      if (innerText.value) {
        innerText.value.innerText = newValue
      }
      if (textContent.value) {
        textContent.value.textContent = newValue
      }
    },
    { immediate: true },
  )
  pInnerHTML = document.getElementById('innerHTML')
  pInnerText = document.getElementById('innerText')
  pTextContent = document.getElementById('textContent')
})
</script>

<template>
  <main>
    <p>Input your text</p>
    <textarea v-model="content"></textarea>
    <fieldset>
      <legend>innerHTML</legend>
      <p v-html="content"></p>
    </fieldset>
    <fieldset>
      <legend>innerText</legend>
      <p ref="innerText"></p>
    </fieldset>
    <fieldset>
      <legend>textContent</legend>
      <p ref="textContent"></p>
    </fieldset>
    <fieldset id="plain">
      <legend>Plain Javascript</legend>
      <textarea @change="changeText"></textarea>
      <fieldset>
        <legend>innerHTML</legend>
        <p id="innerHTML"></p>
      </fieldset>
      <fieldset>
        <legend>innerText</legend>
        <p id="innerText"></p>
      </fieldset>
      <fieldset>
        <legend>textContent</legend>
        <p id="textContent"></p>
      </fieldset>
    </fieldset>
  </main>
</template>

<style lang="scss" scoped>
textarea {
  width: 40%;
  min-height: 100px;
}
#plain p {
  width: 100%;
  border: 1px solid black;
  margin-top: 10px;
  min-height: 20px;
}
</style>
