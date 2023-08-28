<template>
   <div class="bg" :style="{ background: gradientColor }">
      <div class="wrapper">
         <div class="gradient-box" :style="{ background: gradientColor }"></div>
         <div class="option-box">
            <div class="direction">
               <span> Direction </span>
               <div>
                  <select @change="handleChange" name="direction">
                     <option
                        v-for="item in directionOpts"
                        :key="item.value"
                        :value="item.value"
                     >
                        {{ item.label }}
                     </option>
                  </select>
               </div>
            </div>
            <div class="colors">
               <span> Colors </span>
               <div>
                  <input
                     type="color"
                     name="firstColor"
                     style="margin-right: 8px"
                     :value="firstColor"
                     @input="handleChange"
                  />
                  <input
                     type="color"
                     name="secondColor"
                     :value="secondColor"
                     @input="handleChange"
                  />
               </div>
            </div>
         </div>
         <div class="description">background: {{ gradientColor }}</div>
         <div class="btn-box">
            <button @click="handleRefresh">Refresh Code</button>
            <button @click="handleCopy">
               {{ btnText }}
            </button>
         </div>
      </div>
   </div>
</template>

<script lang="ts" setup>
import "./app.less"
import { TDirectionOptions } from "./types"
import { ref, computed } from "vue"
const directionOpts: TDirectionOptions = [
  {
    value: "to top",
    label: "Top"
  },
  {
    value: "to right top",
    label: "Right top"
  },
  {
    value: "to right",
    label: "Right"
  },
  {
    value: "to right bottom",
    label: "Right bottom"
  },
  {
    value: "to bottom",
    label: "Bottom"
  },
  {
    value: "to left bottom",
    label: "Left bottom"
  },
  {
    value: "to left",
    label: "Left"
  },
  {
    value: "to left top",
    label: "Left top"
  }
]

const InitFirstColor = "#977DFE"
const InitSecondColor = "#6878FF"
const direction = ref<string>("to top")
const btnText = ref<string>("Copy Code")
const firstColor = ref<string>(InitFirstColor)
const secondColor = ref<string>(InitSecondColor)
const gradientColor = computed(
  () =>
         `linear-gradient( ${direction.value}, ${firstColor.value}, ${secondColor.value} )`
)
const handleChange = (e: Event) => {
  const { name, value } = e.target as HTMLInputElement
  switch (name) {
    case "firstColor":
      firstColor.value = value
      break
    case "secondColor":
      secondColor.value = value
      break
    case "direction":
      direction.value = value
      break
    default:
      break
  }
}

const handleRefresh = () => {
  firstColor.value = InitFirstColor
  secondColor.value = InitSecondColor
}
const handleCopy = () => {
  navigator.clipboard.writeText(`background: ${gradientColor.value}`)
  btnText.value = "Code Copied"
  const timer = setTimeout(() => {
    btnText.value = "Copy Code"
    clearTimeout(timer)
  }, 800)
}
</script>
