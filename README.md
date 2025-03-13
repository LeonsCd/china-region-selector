# China Region Selector

## 目的

实现一个支持模糊搜索并能直接定位到位置的级联下拉框。

## 背景

市面上的大多数搜索功能通常会弹出一个搜索结果窗口供用户选择，但我们的目标是能够直接在原位置定位（根据老板的要求）。

## 项目结构

- **pca-code.json**: 保存了来自 [Administrative-divisions-of-China](https://github.com/modood/Administrative-divisions-of-China) 的地区数据。
- **index.vue**: 使用该数据的代码。
- **cascader**: 基于 arco-design 源码进行微调，以实现我们的目标。

## 功能

- 支持模糊搜索
- 直接定位到选定位置

