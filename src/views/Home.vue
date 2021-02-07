<template>
  <div class="mind">
    <div>
      <div class="row">
        截屏
        <div><button @click="screenShot()">截图</button></div>
      </div>
      <div class="row">
        换主题
        <select @change="setTheme">
          <option value="">default</option>
          <option value="primary" selected="selected">primary</option>
          <option value="warning">warning</option>
          <option value="danger">danger</option>
          <option value="success">success</option>
          <option value="info">info</option>
          <option value="greensea">greensea</option>
          <option value="nephrite">nephrite</option>
          <option value="belizehole">belizehole</option>
          <option value="wisteria">wisteria</option>
          <option value="asphalt">asphalt</option>
          <option value="orange">orange</option>
          <option value="pumpkin">pumpkin</option>
          <option value="pomegranate">pomegranate</option>
          <option value="clouds">clouds</option>
          <option value="asbestos">asbestos</option>
        </select>
      </div>
      <div class="row">
        操作节点
        <div><button @click="toggleEditable">设置为只读</button></div>
        <div><button @click="addNode">添加节点</button></div>
        <div><button @click="addImgNode">添加图片节点</button></div>
        <div><button @click="removeNode">删除节点</button></div>
      </div>
      <div class="row">
        放大缩小
        <div><button ref="zoomIn" @click="zoomIn">放大</button></div>
        <div><button ref="zoomOut" @click="zoomOut">缩小</button></div>
      </div>
    </div>
    <div class="container" id="jsmind-container" />
    <div style="display: none">
      <input class="file" type="file" ref="imageChooser" accept="image/*" />
    </div>
  </div>
</template>

<script>
import jsMind from '../utils/jsmind'
window.jsMind = jsMind
// eslint-disable-next-line import/first
require('../utils/jsmind.draggable')
require('../utils/jsmind.screenshot')

export default {
  name: 'App',

  data () {
    return {
      xMind: null,
      mind: {},
      imageChooser: null
    }
  },

  mounted () {
    this.setMind()
    this.$nextTick(() => {
      this.imageChooser = this.$refs.imageChooser

      this.imageChooser.addEventListener(
        'change',
        (e) => {
          if (!e || !window.FileReader) return // 看支持不支持FileReader
          const reader = new FileReader()
          const file = e.target.files && e.target.files[0]
          const that = this // 注意this指向问题，onloadend内this指的是file文件

          // reader.onloadend = function () {
          //   console.log('dd', this.result)
          // }
          reader.onloadend = function (e) {
            const selectedNode = that.xMind.get_selected_node() // that指的是当前组件
            const nodeid = jsMind.util.uuid.newid()
            const topic = ''
            const data = {
              'background-image': e.target.result,
              width: '100',
              height: '100'
            }
            that.xMind.add_node(selectedNode, nodeid, topic, data)
          }
          if (file) {
            reader.readAsDataURL(e.target.files[0])
          }
        },
        false
      )
    })
  },

  methods: {
    setMind () {
      this.mind = {
        meta: {
          name: 'my-jsMind'
        },
        format: 'node_array',
        data: [
          {
            id: 'root',
            isroot: true,
            topic: '2021年季度重点工作任务',
            'foreground-color': '#ffffff'
          },
          {
            id: 'sub1',
            parentid: 'root',
            topic: '一季度',
            'background-color': 'red'
          },
          { id: 'sub11', parentid: 'sub1', topic: 'APP4.0' },
          { id: 'sub12', parentid: 'sub1', topic: 'APP4.0' },
          { id: 'sub2', parentid: 'root', topic: '二季度' },
          { id: 'sub3', parentid: 'root', topic: '三季度' },
          { id: 'sub4', parentid: 'root', topic: '四季度' }
        ]
      }
      const option = {
        container: 'jsmind-container',
        editable: true,
        theme: 'primary'
      }
      this.xMind = jsMind.show(option, this.mind) // 第一个参数是思维导图设置选项，第二个参数是思维导图的详细内容
      // this.xMind.disable_edit() // 不允许修改，只读状态
    },
    getSelectedNodeid () {
      const selectedNode = this.xMind.get_selected_node()
      if (selectedNode) {
        return selectedNode.id
      } else {
        return null
      }
    },
    screenShot () {
      this.xMind.screenshot.shootDownload()
    },
    setTheme (e) {
      this.xMind.set_theme(e.target.value)
    },
    toggleEditable (e) {
      const editable = this.xMind.get_editable() // 可编辑
      if (editable) {
        this.xMind.disable_edit() // 不允许修改，只读状态
        e.innerHTML = '可修改'
      } else {
        this.xMind.enable_edit()
        e.innerHTML = '不可修改'
      }
    },
    addNode () {
      const selectedNode = this.xMind.get_selected_node() // as parent of new node
      if (!selectedNode) {
        return alert('please select a node first.')
      }

      const nodeid = jsMind.util.uuid.newid()
      const topic = ' '
      this.xMind.add_node(selectedNode, nodeid, topic)
    },
    addImgNode () {
      const selectedNode = this.xMind.get_selected_node() // as parent of new node
      if (!selectedNode) {
        return alert('please select a node first.')
      }

      this.imageChooser.focus()
      this.imageChooser.click()
    },
    removeNode () {
      const selectedId = this.getSelectedNodeid()
      if (!selectedId) {
        alert('请选择要删除的节点')
      }
      this.xMind.remove_node(selectedId)
    },
    zoomIn () {
      if (this.xMind.view.zoomIn()) {
        this.$refs.zoomOut.disabled = false
      } else {
        this.$refs.zoomIn.disabled = true
      }
    },
    zoomOut () {
      if (this.xMind.view.zoomOut()) {
        this.$refs.zoomIn.disabled = false
      } else {
        this.$refs.zoomOut.disabled = true
      }
    }
  }
}
</script>

<style lang="scss">
.mind {
  display: flex;
  justify-content: flex-start;
  .row {
    margin-top: 20px;
  }
  .container {
    flex: 1;
  }
}
#jsmind-container {
  width: 100%;
  height: 800px;
  background-color: #f2f2f2;
}
input {
  caret-color: red; // 设置光标的颜色
}
</style>
