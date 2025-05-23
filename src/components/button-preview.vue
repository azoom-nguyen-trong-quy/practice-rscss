<script setup>
import { ref, computed } from 'vue'

const ConfigVariant = {
  ICON: 1,
  PREPEND_ICON: 2,
  APPEND_ICON: 3,
  STACKED: 4,
}

const styleVariants = ['Default', 'Outlined', 'Tonal', 'Text', 'Plain']

const configVariants = ref([
  {
    name: 'Icon',
    value: ConfigVariant.ICON,
    checked: false,
  },
  {
    name: 'Prepend icon',
    value: ConfigVariant.PREPEND_ICON,
    checked: false,
  },
  {
    name: 'Append icon',
    value: ConfigVariant.APPEND_ICON,
    checked: false,
  },
  {
    name: 'Stacked',
    value: ConfigVariant.STACKED,
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
  //Rule xử lý nếu click vào Icon thì sẽ uncheck các variant khác
  if (variant.value === ConfigVariant.ICON) {
    configVariants.value.forEach((item) => {
      if (item.value !== ConfigVariant.ICON) {
        item.checked = false
      }
    })
  } else {
    //nếu uncheck thì không xử lý gì cả
    if (!variant.checked) {
      return
    }

    const prependVariant = configVariants.value.find(
      (item) => item.value === ConfigVariant.PREPEND_ICON,
    )
    const appendVariant = configVariants.value.find(
      (item) => item.value === ConfigVariant.APPEND_ICON,
    )
    const stackedVariant = configVariants.value.find((item) => item.value === ConfigVariant.STACKED)

    configVariants.value.forEach((item) => {
      switch (item.value) {
        case ConfigVariant.ICON: //Không click vào icon thì mặc định uncheck
          item.checked = false
          break
        case ConfigVariant.PREPEND_ICON:
          //Nếu stacked và append đang check thì uncheck prepend
          if (stackedVariant.checked && variant.value === ConfigVariant.APPEND_ICON) {
            item.checked = false
          } else if (stackedVariant.checked) {
            //Nếu stacked đang check thì mặc định check prepend
            item.checked = true
          }
          break
        case ConfigVariant.APPEND_ICON:
          //nếu stacked check và prepend đang check thì uncheck append
          if (stackedVariant.checked && prependVariant.checked) {
            item.checked = false
          }
          break
        case ConfigVariant.STACKED:
          break
      }
    })
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
      <div class="title">Configuration</div>
      <div
        class="option"
        v-for="(variant, index) in configVariants"
        :key="index"
        :checked="variant.checked"
      >
        <i :class="[variant.checked ? '-checked' : '-uncheck', 'check']" />
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
  grid-template-columns: 3fr 1fr;
  color: #fff;

  > .style-variant {
    grid-area: header;
    display: flex;
    padding: 8px;
    gap: 8px;
    background-color: #424242;
    font-size: 12px;
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
    font-size: 14px;
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
    padding: 16px;
  }

  > .config-variant > .title,
  > .config-variant > .option {
    height: 40px;
  }

  > .config-variant > .title {
    color: rgb(255 255 255 / 70%);
    font-size: 14px;
  }

  > .config-variant > .option {
    font-size: 16px;
    display: flex;
    gap: 4px;
    align-items: center;
    position: relative;
  }

  > .config-variant > .option > .check {
    width: 24px;
    height: 24px;
    background-color: #fff;
  }

  > .config-variant > .option > .check.-checked {
    mask-image: url('@/assets/check.svg');
  }

  > .config-variant > .option > .check.-uncheck {
    mask-image: url('@/assets/uncheck.svg');
  }

  > .config-variant > .option > input {
    width: 28px;
    height: 28px;
    position: absolute;
    opacity: 0;
    cursor: pointer;
  }
}
</style>
