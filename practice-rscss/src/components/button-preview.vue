<script setup>
import { ref, computed } from 'vue'

const styleVariants = ['Default', 'Outlined', 'Tonal', 'Text', 'Plain']

const configVariants = ref([
  {
    name: 'Icon',
    checked: false,
  },
  {
    name: 'Prepend icon',
    checked: false,
  },
  {
    name: 'Append icon',
    checked: false,
  },
  {
    name: 'Stacked',
    checked: false,
  },
])

const styleVariant = ref('-default')
const configVariant = computed(() => {
  return configVariants.value
    .filter((variant) => variant.checked)
    .map((variant) => generateVariantClass(variant.name))
    .join(' ')
})

const generateVariantClass = (variant) => {
  if (!variant) return ''

  return `-${variant.replace(' ', '-').toLowerCase()}`
}

const onClickStyleVariant = (variant) => {
  styleVariant.value = generateVariantClass(variant)
}

const onClickConfigVariant = (variant) => {
  if (variant.name === 'Icon') {
    configVariants.value.forEach((item) => {
      if (item.name !== 'Icon') {
        item.checked = false
      }
    })
  } else {
    const iconVariant = configVariants.value.find((item) => item.name === 'Icon')
    if (iconVariant) {
      iconVariant.checked = false
    }
  }
}
</script>

<template>
  <div class="button-preview">
    <div class="style-variant">
      <div
        :class="[
          'option',
          {
            '-active': styleVariant === `-${variant.toLowerCase()}`,
          },
        ]"
        v-for="variant in styleVariants"
        :key="variant"
        @click="onClickStyleVariant(variant)"
      >
        {{ variant }}
      </div>
    </div>
    <div :class="['preview-content', styleVariant, configVariant]">
      <div class="icon -none-text">
        <img src="@/assets/logo.svg" width="24" height="24px" />
      </div>

      <div class="button-box">
        <img class="icon -prepend" src="@/assets/logo.svg" width="18" height="18" />
        <div class="text">BUTTON</div>
        <img class="icon -append" src="@/assets/logo.svg" width="18" height="18" />
      </div>
    </div>
    <div class="config-variant">
      <div class="option" v-for="(variant, index) in configVariants" :key="index">
        <input type="checkbox" @change="onClickConfigVariant(variant)" v-model="variant.checked" />
        <span>{{ variant.name }}</span>
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.button-preview {
  width: 900px;
  background-color: rgb(33, 33, 33);
  border: 1px solid #333;
  border-radius: 4px;
  display: grid;
  grid-template-areas:
    'header header'
    'content sidebar';
  color: #fff;

  > .style-variant {
    grid-area: header;
    display: flex;
    padding: 8px;
    gap: 8px;
    background-color: #424242;
  }

  > .style-variant > .option {
    padding: 4px 12px;
    border-radius: 4px;
    cursor: pointer;
  }

  > .style-variant > .option:hover {
    background-color: #333030;
  }

  > .style-variant > .option.-active {
    background-color: #fff;
    background-color: #606060;
  }

  > .preview-content {
    grid-area: content;
    border-right: 1px solid #333;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 300px;
  }

  > .preview-content > .icon {
    width: 48px;
    height: 48px;
    display: none;
    justify-content: center;
    align-items: center;
    border-radius: 50%;
  }

  > .preview-content > .button-box {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 8px;
    padding: 10px 16px;
    border-radius: 4px;
    cursor: pointer;
  }

  > .preview-content.-default > .button-box,
  > .preview-content.-default > .icon {
    box-shadow: 3px 3px 5px 0px rgba(0, 0, 0, 0.75);
  }

  > .preview-content.-outlined > .button-box,
  > .preview-content.-outlined > .icon {
    border: 1px solid #fff;
  }

  > .preview-content.-tonal > .button-box,
  > .preview-content.-tonal > .icon {
    background-color: rgb(47, 47, 47);
  }

  > .preview-content.-plain > .button-box,
  > .preview-content.-plain > .icon {
    opacity: 0.62;
  }
  > .preview-content.-icon > .button-box {
    display: none;
  }

  > .preview-content.-icon > .icon {
    display: flex;
  }

  > .preview-content.-prepend-icon > .button-box > .icon.-prepend,
  > .preview-content.-append-icon > .button-box > .icon.-append {
    display: block;
  }

  > .preview-content.-stacked > .button-box {
    flex-direction: column;
  }

  > .preview-content > .button-box > .icon {
    display: none;
  }

  > .config-variant {
    grid-area: sidebar;
  }
}
</style>
