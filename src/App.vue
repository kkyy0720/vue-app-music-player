<!--
Author: Yuki
Date: 2022-04-10 20:15:36
LastEditTime: 2022-04-20 13:59:20
Description: 入口
FilePath: \vite-music-player\src\App.vue
-->

<template>
  <!-- v-columnMatch指令 js响应式布局，计算gird列数 -->
  <div id="view" v-columnMatch>
    <router-view />
  </div>
</template>

<style lang="less">
* {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  -webkit-overflow-scrolling: touch;
  -webkit-tap-highlight-color: transparent;
  overscroll-behavior-y: none;

  padding: 0;
  margin: 0;
  box-sizing: border-box;
  background: transparent;
  text-decoration: none;
  list-style: none;
  line-height: 1.6;
  z-index: 0;
}

html {
  scrollbar-color: rgba(0, 0, 0, 0.2) #fff;
  scrollbar-width: auto;
  width: 100vw;
  position: relative;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;

  body {
    width: 100%;
    // 播放器Y偏移
    --player-translate: calc(100vh - 72px - 72px);
  }

  a {
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0) !important;
  }
}

.disable-hover {
  pointer-events: none;
}

.user-not-select {
  -webkit-touch-callout: none;
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
}

:root {
  // 左侧导航栏宽度
  --nav-left-width: 240px;

  // grid布局列数
  --column-count: 5;

  // 头部高度
  --header-height: 72px;

  // 动画进度
  --animation-ratio: 0;

  // 动画触发点，一般是100vh高度
  --animation-target: 0;

  // 主题色
  --theme-color: rgba(250, 35, 59, 1);

  // gap间隙
  --grid-gap: 24px;

  // 页面两边距
  --page-spacing: 32px;

  // @media screen and (min-width: 1200px) {
  // }

  // @media screen and (max-width: 1200px) {
  // }

  // @media screen and (max-width: 1068px) {
  // }

  @media screen and (max-width: 768px) {
    --grid-gap: 10px;
    --page-spacing: 16px;
  }

  // @media screen and (max-width: 628px) {
  // }
}

.flex-center {
  display: flex;
  justify-content: center;
  align-items: center;
}

.flex-al-end {
  display: flex;
  justify-content: center;
  align-items: flex-end;
}

.spacing {
  padding: 32px var(--page-spacing) 0;
}

::-webkit-scrollbar {
  width: 14px;
  height: 14px;
  background-color: #fff;
}

/*定义滚动条轨道 内阴影+圆角*/
::-webkit-scrollbar-track {
  background-color: transparent;
}

/*定义滑块 内阴影+圆角*/
::-webkit-scrollbar-thumb {
  border-radius: 1px;
  background-color: rgba(0, 0, 0, 0.2);
}

::-webkit-input-placeholder {
  /* WebKit browsers */
  color: rgba(255, 255, 255, 0.35);
}

:-moz-placeholder {
  /* Mozilla Firefox 4 to 18 */
  color: rgba(255, 255, 255, 0.35);
}

::-moz-placeholder {
  /* Mozilla Firefox 19+ */
  color: rgba(255, 255, 255, 0.35);
}

:-ms-input-placeholder {
  /* Internet Explorer 10+ */
  color: rgba(255, 255, 255, 0.35);
}
</style>
