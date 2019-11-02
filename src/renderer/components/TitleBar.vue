<template>

    <div id="app-title-bar" class="app-title-bar">
        <div class="resize-handle resize-handle-top" />
        <div class="resize-handle resize-handle-left" />
        <!-- <div class="title">{{ title }}</div> -->
        <img src="@/assets/rosalina.png" alt="Logo" class="logo">
        <div class="window-controls">
          <button aria-label="minimize" tabIndex="-1" class="window-control window-minimize"
              v-on:click="windowMinimize">
            <svg aria-hidden="true" version="1.1" width="10" height="10">
              <path d="M 0,5 10,5 10,6 0,6 Z" />
            </svg>
          </button>
          <button aria-label="maximize" tabIndex="-1" class="window-control window-maximize"
            v-on:click="windowMaximize">
            <svg aria-hidden="true" version="1.1" width="10" height="10">
              <path d="M 0,0 0,10 10,10 10,0 Z M 1,1 9,1 9,9 1,9 Z" />
            </svg>
          </button>
          <button aria-label="close" tabIndex="-1" class="window-control window-close"
            v-on:click="windowClose">
            <svg aria-hidden="true" version="1.1" width="10" height="10">
              <path d="M 0,0 0,0.7 4.3,5 0,9.3 0,10 0.7,10 5,5.7 9.3,10 10,10 10,9.3 5.7,5 10,0.7 10,0 9.3,0 5,4.3 0.7,0 Z" />
            </svg>
          </button>
      </div>
    </div>

</template>

<script>
  import {remote} from 'electron'
  export default {
    name: 'title-bar',
    props: ['title'],
    methods: {
      windowMinimize: function () {
        var win = remote.getCurrentWindow()
        win.minimize()
      },
      windowMaximize: function () {
        var win = remote.getCurrentWindow()
        return win.isMaximizable() ? win.isMaximized() ? win.unmaximize() : win.maximize() : null
      },
      windowClose: function () {
        var win = remote.getCurrentWindow()
        win.close()
      }
    }
  }
</script>

<style scoped>
#app-title-bar {
  z-index: 999;
  height: 28px;
  background: rgb(61, 64, 69);
  /* background: transparent; */
  border-bottom: transparent;
  -webkit-app-region: drag;
  flex-grow: 0;
  flex-shrink: 0;
  width: 100%;
  display: flex;
  flex-direction: row;
  /* color: #24292e; */
  color: transparent;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", Arial, sans-serif;
  font-size: 12px;
  position: absolute;
}

/*resize handler*/
#app-title-bar .resize-handle {
  position: absolute;
  top: 0px;
  left: 0px;
  -webkit-app-region: no-drag;
}

#app-title-bar .resize-handle.resize-handle-left {
  width: 3px;
  height: 28px;
}

#app-title-bar .resize-handle.resize-handle-top {
  width: 100%;
  height: 3px;
}

/*icon*/
#app-title-bar img.icon {
  height: 16px;
  width: 16px;
  margin: 6px;
}

/*title*/
#app-title-bar .logo {
  margin-left: 6px;
  margin-bottom: 2px;
  margin-top: 4px;
  color: rgb(202, 206, 221);
  -webkit-app-region: no-drag;
}

/*toolbar button*/
#app-title-bar .toolbar-button {
  min-width: 0;
  position: relative;
}

#app-title-bar .toolbar-button > button {
  -webkit-appearance: none;
  border: none;
  box-shadow: none;
  background: transparent;
  border-radius: 0;
  text-align: left;
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
}

#app-title-bar .toolbar-button > button:active {
  box-shadow: none;
}

#app-title-bar .toolbar-button > button:focus {
  background-color: #2f363d;
  outline-offset: -4px;
  border-color: black;
  box-shadow: none;
}

#app-title-bar .toolbar-button > button:focus .progress {
  background: #444d56;
}

#app-title-bar .toolbar-button > button:focus:not(.focus-ring) {
  outline: none;
  background-color: transparent;
}

#app-title-bar .toolbar-button > button:focus:not(.focus-ring) .progress {
  background: #2f363d;
}

#app-title-bar .toolbar-button > button:not(:disabled):hover {
  background-color: #2f363d;
  color: #fff;
  border-color: black;
}

#app-title-bar .toolbar-button > button:not(:disabled):hover .description {
  color: #959da5;
}

#app-title-bar .toolbar-button > button:not(:disabled):hover .progress {
  background: #444d56;
}

#app-title-bar .toolbar-button > button {
  position: relative;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 10px;
  margin: 0;
  overflow: hidden;
  background-color: transparent;
  color: #fff;
  border-right: 1px solid black;
}

#app-title-bar .toolbar-button > button .icon {
  flex-shrink: 0;
  margin-right: 10px;
  position: relative;
}

#app-title-bar .toolbar-button > button .dropdownArrow {
  width: 9px;
  height: 13px;
  flex-shrink: 0;
  position: relative;
}

#app-title-bar .toolbar-button > button .text {
  display: flex;
  flex-direction: column;
  flex-grow: 1;
  min-width: 0;
  margin-right: 10px;
  position: relative;
}

#app-title-bar .toolbar-button > button .title {
  font-weight: 600;
  position: relative;
}

#app-title-bar .toolbar-button > button .description {
  color: #959da5;
  font-size: 11px;
  position: relative;
}

#app-title-bar .toolbar-button > button .title, .toolbar-button > button .description {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

#app-title-bar .toolbar-button > button .progress {
  position: absolute;
  top: 0px;
  left: 0px;
  width: 100%;
  height: 100%;
  background: #2f363d;
  transform-origin: left;
  pointer-events: none;
  transition: transform 0.3s cubic-bezier(0.23, 1, 0.32, 1);
}

#app-title-bar .toolbar-button.has-progress > button:disabled {
  opacity: 1;
}

/*button component*/
#app-title-bar .button-component {
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen", "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue", Arial, sans-serif;
  font-size: 12px;
  padding: 0 10px;
  border: 1px solid #e1e4e8;
  height: 25px;
  color: #24292e;
  background-color: #fafbfc;
  border-radius: 2px;
}

#app-title-bar .button-component:not(:disabled):hover {
  border-color: #d1d5da;
  background-color: #fff;
}

#app-title-bar .button-component:focus {
  background-color: #fff;
  border-color: #d1d5da;
  box-shadow: 0 0 0 1px rgba(225, 228, 232, 0.75);
}

#app-title-bar .button-component:disabled {
  opacity: 0.6;
}

#app-title-bar .button-component .octicon {
  vertical-align: middle;
}

#app-title-bar .button-component[type='submit'] {
  background-color: #0366d6;
  color: #fff;
  border: 1px solid #0366d6;
}

#app-title-bar .button-component[type='submit']:not(:disabled):hover {
  border-color: #0366d6;
  background-color: #0372ef;
}

#app-title-bar .button-component[type='submit']:focus {
  border: 1px solid #005cc5;
  background-color: #0372ef;
  box-shadow: 0 0 0 2px rgba(3, 102, 214, 0.25);
}


/*dropdown*/
#app-title-bar .toolbar-dropdown {
  min-width: 0;
}

#app-title-bar .toolbar-dropdown > .toolbar-button {
  width: 100%;
  height: 100%;
}

#app-title-bar .toolbar-dropdown.open > .toolbar-button > button {
  color: #24292e;
  background-color: #fff;
  border-color: #fff;
}

#app-title-bar .toolbar-dropdown.open > .toolbar-button > button .description {
  color: #6a737d;
}

#app-title-bar .toolbar-dropdown.open > .toolbar-button > button .progress {
  background-color: #e1e4e8;
}

#app-title-bar .toolbar-dropdown:not(.open) .menu-item .menu-label {
  opacity: 0.6;
}

/*menubar*/
#app-title-bar #app-menu-bar {
  display: flex;
  -webkit-app-region: no-drag;
  max-width: calc(100% - 163px);
  overflow: hidden;
}

#app-title-bar #app-menu-bar .toolbar-button > button {
  padding: 0 10px;
  border: 0;
}

#app-title-bar #app-menu-bar .toolbar-button > button .access-key.highlight {
  text-decoration: underline;
}

#app-title-bar #app-menu-bar .toolbar-dropdown:not(.open) > .toolbar-button > button {
  color: #959da5;
}

#app-title-bar #app-menu-bar .toolbar-dropdown:not(.open) > .toolbar-button > button:hover, #app-menu-bar .toolbar-dropdown:not(.open) > .toolbar-button > button:focus {
  color: #fff;
}

#app-title-bar #app-menu-bar #foldout-container .foldout {
  background: transparent;
  pointer-events: none;
}

#app-title-bar #app-menu-bar #foldout-container .foldout .menu-pane {
  background: #fff;
  pointer-events: all;
}

#app-title-bar #foldout-container {
  z-index: 8;
}

#app-title-bar #foldout-container .overlay {
  background: black;
  opacity: 0.4;
  height: 100%;
  overflow: hidden;
}

#app-title-bar #foldout-container .overlay:focus {
  outline: none;
  border: none;
  box-shadow: none;
}

#app-title-bar #foldout-container .foldout {
  background: #fff;
  color: #24292e;
}

/*menupane*/
#app-title-bar #app-menu-foldout {
  height: 100%;
  display: flex;
}

#app-title-bar .list {
  flex-grow: 1;
  height: 100%;
  min-width: 0;
  overflow: hidden;
}

#app-title-bar .list .ReactVirtualized__Grid {
  background: #fff;
}

#app-title-bar .list-item {
  display: flex;
  flex-direction: row;
  align-items: center;
  width: 100%;
  height: 100%;
}

#app-title-bar .list-item.selected {
  color: #fff;
  background-color: #0366d6;
}

#app-title-bar .list-item.selected:focus {
  color: #fff;
  background-color: #0366d6;
}

#app-title-bar .list-item:focus {
  outline: none;
}

#app-title-bar .menu-pane {
  height: 100%;
  width: 240px;
}

#app-title-bar .menu-pane:not(:first-child) {
  border-left: 1px solid #e1e4e8;
}

#app-title-bar .menu-pane:not(:last-child) .list-item.selected {
  color: #fff;
  background-color: #0366d6;
}

#app-title-bar .menu-pane .ReactVirtualized__Grid:focus {
  outline: none;
}

#app-title-bar .menu-pane .menu-item {
  display: flex;
  align-items: center;
  height: 100%;
  width: 100%;
  min-width: 0;
}

#app-title-bar .menu-pane .menu-item.disabled {
  opacity: 0.3;
}

#app-title-bar .menu-pane .menu-item .menu-label {
  flex-grow: 1;
  margin-left: 10px;
  margin-right: 10px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}

#app-title-bar .menu-pane .menu-item .submenu-arrow {
  flex-shrink: 0;
  opacity: 0.7;
  height: 12px;
  margin-right: 10px;
}

#app-title-bar .menu-pane .menu-item .accelerator {
  flex-shrink: 0;
  margin-right: 10px;
  color: #6a737d;
}

#app-title-bar .list-item.selected .accelerator {
  color: #fff;
}

#app-title-bar .menu-pane .menu-item.checked .menu-label {
  margin-left: 0;
}

#app-title-bar .menu-pane .menu-item.checked .icon {
  flex-grow: 0;
  margin: 2px 0;
}

#app-title-bar .menu-pane .menu-item .access-key.highlight {
  text-decoration: underline;
}

#app-title-bar .menu-pane hr {
  display: block;
  width: 100%;
  border: none;
  height: 1px;
  border-bottom: 1px solid #e1e4e8;
}

#app-title-bar .menu-pane .menu-endblock {
  height: 5px;
}

#app-title-bar .status-icon {
  margin-left: 10px;
  width: 12px;
  height: 12px;
}

#app-title-bar .status-icon svg {
  width: 100%;
  height: 100%;
}
/*window-controls*/
#app-title-bar .window-controls {
  flex-grow: 0;
  flex-shrink: 0;
  margin-left: auto;
  height: 100%;
}

#app-title-bar .window-controls button {
  -webkit-app-region: no-drag;
  display: inline-block;
  position: relative;
  width: 45px;
  height: 100%;
  padding: 0;
  margin: 0;
  overflow: hidden;
  border: none;
  box-shadow: none;
  border-radius: 0;
  color: #a0a0a0;
  background-color: transparent;
  transition: background-color 0.25s ease;
}

#app-title-bar .window-controls button:not(:disabled):focus {
  outline: none;
}

#app-title-bar .window-controls button:not(:disabled):hover {
  background-color: #888;
  color: #fff;
}

#app-title-bar .window-controls button:not(:disabled):hover:active {
  background-color: #666;
  transition: none;
}

#app-title-bar .window-controls button.window-close:not(:disabled):hover {
  background-color: #e81123;
  color: #fff;
}

#app-title-bar .window-controls button.window-close:not(:disabled):hover:active {
  background-color: #bf0f1d;
  transition: none;
}

#app-title-bar .window-controls button svg {
  fill: currentColor;
}

#app-title-bar :not(input):not(textarea), :not(input):not(textarea)::after, :not(input):not(textarea)::before {
  -webkit-user-select: none;
  user-select: none;
  cursor: default;
}

</style>