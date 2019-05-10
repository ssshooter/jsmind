<template>
  <div>
    <div
      class="mind-menu"
      v-if="this.currentNode.id"
    >
      <div>
        <i
          class="iconfont"
          @click="menuState=0"
        >&#xe600;</i>
        <i
          class="iconfont"
          @click="menuState=1"
        >&#xe990;</i>
        <i
          class="iconfont"
          @click="menuState=2"
        >&#xe68a;</i>
        <i
          class="iconfont"
          @click="menuState=3"
        >&#xe633;</i>
      </div>
      <div v-if="menuState === 0">
        <el-form label-width="100px">
          <el-form-item label="背景颜色">
            <el-color-picker v-model="currentNode.data['background-color']"></el-color-picker>
          </el-form-item>
          <el-form-item label="文字颜色">
            <el-color-picker v-model="currentNode.data['foreground-color']"></el-color-picker>
          </el-form-item>
          <el-form-item label="字体大小">
            <el-slider
              :min="16"
              :max="30"
              :step="1"
              v-model="currentNode.data['font-size']"
            ></el-slider>
          </el-form-item>
        </el-form>
      </div>
      <div v-else-if="menuState === 1">
        图标
      </div>
      <div v-else-if="menuState === 2">
        备注
      </div>
      <div v-else-if="menuState === 3">
        标签
      </div>
    </div>
    <div
      id="jsmind_container"
      style="height: 100%; width: 100%;"
    ></div>
    <ContextMenu />
  </div>
</template>

<script>
import jsMind from '../../js/jsmind.js'
import ContextMenu from './ContextMenu'
export default {
  components: { ContextMenu },
  data() {
    return {
      menuState: 0,
      currentNode: {
        data: {
          'background-color': '#ffffff',
          'foreground-color': '#735c45',
          'font-size': '16',
        },
      },
    }
  },
  mounted() {
    var mind = {
      // 3 data formats were supported ...
      // see Documents for more information
      /* 元数据，定义思维导图的名称、作者、版本等信息 */
      meta: {
        name: 'jsMind-demo-tree',
        author: 'hizzgdev@163.com',
        version: '0.2',
      },
      /* 数据格式声明 */
      format: 'node_tree',
      /* 数据内容 */
      data: {
        id: 'root',
        topic: 'jsMind',
        children: [
          {
            id: 'easy',
            topic: 'Easy',
            direction: 'left',
            expanded: false,
            children: [
              {
                id: 'easy1',
                topic: 'Easy to show<i class="iconfont">&#xe600;</i>',
              },
              { id: 'easy2', topic: 'Easy to edit' },
              { id: 'easy3', topic: 'Easy to store' },
              {
                id: 'easy4',
                topic: 'Easy to embed',
                'background-color': '#eee',
              },
            ],
          },
          {
            id: 'open',
            topic: 'Open Source',
            direction: 'right',
            expanded: true,
            children: [
              { id: 'open1', topic: 'on GitHub' },
              { id: 'open2', topic: 'BSD License' },
            ],
          },
          {
            id: 'powerful',
            topic: 'Powerful',
            direction: 'right',
            children: [
              { id: 'powerful1', topic: 'Base on Javascript' },
              { id: 'powerful2', topic: 'Base on HTML5' },
              { id: 'powerful3', topic: 'Depends on you' },
            ],
          },
          {
            id: 'other',
            topic: 'test node',
            direction: 'left',
            children: [
              { id: 'other1', topic: "I'm from local variable" },
              { id: 'other2', topic: 'I can do everything' },
            ],
          },
        ],
      },
    }
    var options = {
      container: 'jsmind_container',
      theme: 'shooter',
      editable: true,
      direction: 'LR',
      support_html:false,
    }
    var jm = new jsMind(options)
    // jm.disable_edit()
    console.log(jm)
    jm.show(mind)

    var newid = function() {
      return (
        new Date().getTime().toString(16) +
        Math.random()
          .toString(16)
          .substr(2)
      ).substr(2, 16)
    }
    var ctn = document.querySelector('#jsmind_container')
    var addChild = document.querySelector('#jm-cm-add_child')
    var addsibling = document.querySelector('#jm-cm-add_sibling')

    addChild.addEventListener('click', e => {
      let current = jm.get_selected_node()
      let newNode = jm.add_node(current, newid(), 'New Node')
      jm.begin_edit(newNode)
    })

    addsibling.addEventListener('click', e => {
      let current = jm.get_selected_node()
      let newNode = jm.insert_node_after(current, newid(), 'New Node')
      jm.begin_edit(newNode)
    })

    document.onkeydown = function(e) {
      if (e.keyCode === 9) e.preventDefault()
    }
    ctn.addEventListener('click', e => {
      if (e.target.tagName !== 'INPUT') {
        console.log(e, jm.get_selected_node())
        jm.update_node_manually(this.currentNode)
      }
      if (e.target.tagName === 'JMNODE') {
        e.preventDefault()
        if (!this.currentNode.isroot) {
          this.currentNode = jm.get_selected_node()
        }
      } else {
        this.currentNode = {
          data: {
            'background-color': '#ffffff',
            'foreground-color': '#735c45',
            'font-size': 16,
          },
        }
      }
    })
    ctn.oncontextmenu = function(e) {
      if (e.target.tagName === 'JMNODE') {
        e.preventDefault()
        let menuBg = document.querySelector('.context-menu')
        let menu = document.querySelector('.contextMenu')
        menuBg.hidden = false
        console.log(jm.get_selected_node())
        menu.style.top = e.pageY + 15 + 'px'
        menu.style.left = e.pageX + 10 + 'px'
      }
    }

    // this.$watch(
    //   'currentNode',
    //   () => {
    //     jm.update_node_manually()
    //   },
    //   {
    //     deep: true,
    //   }
    // )
  },
}
</script>

<style>
@import "../../style/jsmind.css";
</style>
<style>
.mind-menu {
  position: absolute;
  right: 10px;
  top: 10px;
  z-index: 10;
}
jmnodes.theme-shooter jmnode {
  box-shadow: none;
  border: 1px solid rgb(68, 68, 68);
  background-color: #fff;
  color: rgb(115, 92, 69);
}
jmnodes.theme-shooter jmnode:hover {
  box-shadow: 0 0 10px rgb(115, 92, 69);
}
jmnodes.theme-shooter jmnode.selected {
  box-shadow: 0 0 10px #409eff;
}
jmnodes.theme-shooter jmnode.root {
  background-color: rgb(80, 194, 139);
  color: #fff;
}
jmnodes.theme-shooter jmexpander {
}
jmnodes.theme-shooter jmexpander:hover {
}
</style>
