# vue-element-admin

[vue-element-admin](https://github.com/PanJiaChen/vue-element-admin)

vue create cli
`"core-js": "^2.6.5",`

## Sidebar

[路由和侧边栏](https://panjiachen.github.io/vue-element-admin-site/zh/guide/essentials/router-and-nav.html)
Element UI > 组件 > [NavMenu 导航菜单](https://cloud.tencent.com/developer/section/1489896)
[侧边栏](https://www.datatest.org/blog/vue-sidebar)

navbar

```js
  <div class="navbar">
    <hamburger id="hamburger-container" :is-active="sidebar.opened" class="hamburger-container" @toggleClick="toggleSideBar" />
  
  methods: {
    toggleSideBar() {
      this.$store.dispatch('app/toggleSideBar')
    },
```

store\modules\app.js

```js
const actions = {
  toggleSideBar({ commit }) {
    commit('TOGGLE_SIDEBAR')
  },

const mutations = {
  TOGGLE_SIDEBAR: state => {
    state.sidebar.opened = !state.sidebar.opened
```

layout

src\layout\index.vue

```html
<template>
  <div :class="classObj" class="app-wrapper">
      <sidebar class="sidebar-container" />
      <div :class="{hasTagsView:needTagsView}" class="main-container">
        <div :class="{'fixed-header':fixedHeader}">

<div data-v-13877386="" class="app-wrapper openSidebar">
<div data-v-13877386="" class="app-wrapper hideSidebar">
```

src\styles\sidebar.scss

```css
  .hideSidebar {
    .sidebar-container {
      width: 54px !important;
    }

    .main-container {
      margin-left: 54px;
    }
```

```js
 computed: {
    ...mapState({
      sidebar: state => state.app.sidebar,
    }),
    classObj() {
      return {
        hideSidebar: !this.sidebar.opened,
        openSidebar: this.sidebar.opened,
```

src\styles\sidebar.scss

當菜單收縮時element會給菜單添加el-menu--collapse類，這時候把文字和下拉圖標隱藏掉即可

styles\index.scss

```
@import './sidebar.scss';
```

src\styles\sidebar.scss
```js
    .el-menu--collapse {
      .el-submenu {
        &>.el-submenu__title {
          &>span {
            height: 0;
            width: 0;
            overflow: hidden;
            visibility: hidden;
            display: inline-block;
          }
        }
      }
    }
```

router\index.js

```js
import Layout from '@/layout'

  {
    path: '/',
    component: Layout,
    redirect: '/dashboard',
    children: [
      {
        path: 'dashboard',
        component: () => import('@/views/dashboard/index'),
        name: 'Dashboard',
        meta: { title: 'Dashboard', icon: 'dashboard', affix: true }
      }
    ]
  },
```

src\layout\components\index.js

```js
export { default as Sidebar } from './Sidebar/index.vue'
```

src\layout\components\Sidebar\index.vue

```js
import variables from '@/styles/variables.scss'
<sidebar-item v-for="route in permission_routes" :key="route.path" :item="route" :base-path="route.path" />

    ...mapGetters([
      'permission_routes',
      'sidebar'
    ]),
    variables() {
      return variables
    },
```

/styles/variables.scss

```css
$sideBarWidth: 210px;
// the :export directive is the magic sauce for webpack
// https://www.bluematador.com/blog/how-to-share-variables-between-js-and-sass
:export {
  sideBarWidth: $sideBarWidth;
}
```

src\store\modules\permission.js

```js
// constantRoutes 與 asyncRoutes 決定 sidebar-item 內容
import { asyncRoutes, constantRoutes } from '@/router'

const mutations = {
  SET_ROUTES: (state, routes) => {
    state.addRoutes = routes
    state.routes = constantRoutes.concat(routes) // constantRoutes 在前 asyncRoutes 在後
  }
}
```

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
