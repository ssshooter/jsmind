<template>
  <div
    class="context-menu bg"
    @click="hide"
    hidden
  >
    <ul class="contextMenu">
      <li id="jm-cm-add_child">
        <span>插入子节点</span>
        <span>tab</span>
      </li>
      <li id="jm-cm-add_sibling">
        <span>插入同级节点</span>
        <span>enter</span>
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  methods: {
    hide() {
      let menuBg = document.querySelector('.context-menu')
      menuBg.hidden = true
    },
  },
}
</script>

<style>
.bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 99;
}
ul.contextMenu {
  position: fixed;
  list-style: none;
  margin: 0;
  padding: 0;
  font: 300 15px 'Roboto', sans-serif;
  color: #333;
  box-shadow: 0 12px 15px 0 rgba(0, 0, 0, 0.2);
}

ul.contextMenu * {
  transition: color 0.4s, background 0.4s;
}

ul.contextMenu li {
  min-width: 150px;
  overflow: hidden;
  white-space: nowrap;
  padding: 6px 10px;
  background-color: #fff;
  border-bottom: 1px solid #ecf0f1;
}

ul.contextMenu li a {
  color: #333;
  text-decoration: none;
}

ul.contextMenu li:hover {
  background-color: #ecf0f1;
}

ul.contextMenu li:first-child {
  border-radius: 5px 5px 0 0;
}

ul.contextMenu li:last-child {
  border-bottom: 0;
  border-radius: 0 0 5px 5px;
}

ul.contextMenu li span:last-child {
  float: right;
}
</style>
