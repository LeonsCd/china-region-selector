<template>
  <Cascader
    ref="cascaderRef"
    :options="options"
    :field-names="{
      value: 'name',
      label: 'name',
    }"
    placeholder="请选择所属区域"
    allow-search
    allow-clear
    path-mode
    v-bind="props"
    :filter-option="filterOption"
    :trigger-props="{ contentClass }"
    @search="handleRegionSearch"
  />
</template>

<script setup lang="ts">
  import { useAttrs, ref, nextTick } from 'vue'
  import { pinyin } from 'pinyin-pro'
  import options from './pca-code.json'
  import Cascader from './cascader/cascader.vue'

  const props = useAttrs()
  const cascaderRef = ref()
  const contentClass = 'customRegionCascader'

  const filterOption = (inputValue: string, option: any) => {
    // 将选项名称转换为拼音
    const pinyinName = pinyin(option.name, {
      toneType: 'none',
      type: 'array',
    }).join('')

    // 检查输入值是否在选项名称或其拼音中
    return (
      option.name.includes(inputValue) ||
      pinyinName.includes(inputValue.toLowerCase())
    )
  }

  // 定位并展开下一级
  const locateAndClickOption = async (
    panel: Element,
    columnIndex: number,
    optionName: string
  ) => {
    const column = panel.getElementsByClassName('arco-cascader-panel-column')[
      columnIndex
    ]
    const option = column.querySelector(
      `li[title="${optionName}"]`
    ) as HTMLElement
    const scrollbar = column.querySelector('.arco-scrollbar-container')

    if (scrollbar && option) {
      scrollbar.scrollTo({
        top: option.offsetTop,
      })
      if (columnIndex < 2) option.click()
    }
  }

  // 自定义搜索
  const handleRegionSearch = async (value: string) => {
    if (!value || !cascaderRef.value.filteredLeafOptions?.length) return

    const panel = document.querySelector(`.${contentClass}`)
    if (!panel) return

    const { pathValue } = cascaderRef.value.filteredLeafOptions[0]

    // 依次定位省市县
    pathValue.forEach((name: string, i: number) => {
      nextTick(() => {
        locateAndClickOption(panel, i, name)
      })
    })
  }
</script>

<style scoped></style>
