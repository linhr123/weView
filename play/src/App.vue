<template>
  <h1>Demo</h1>

  <!-- Menu组件 start -->
  <h3>• Menu导航菜单</h3>
  <w-menu defaultTo="/git" mode="horizontal" @select="handlerSelect">
    <w-sub-menu to="basic">
      <template #title>入门</template>
      <w-menu-item-group>
        <template #title>基础语言</template>
        <w-menu-item to="/html">HTML</w-menu-item>
        <w-menu-item to="/css">CSS</w-menu-item>
        <w-menu-item to="/javascript">JavaScript</w-menu-item>
      </w-menu-item-group>
      <w-menu-item-group title="框架">
        <w-sub-menu to="Vue">
          <template #title>Vue框架</template>
          <w-menu-item to="/vue2">Vue2</w-menu-item>
          <w-menu-item to="/vue3">Vue3</w-menu-item>
        </w-sub-menu>
        <w-menu-item to="/react">React</w-menu-item>
      </w-menu-item-group>
    </w-sub-menu>

    <div class="flex-grow"></div>
    <w-menu-item to="/webpack">Webpack</w-menu-item>
    <w-menu-item to="/vite">Vite</w-menu-item>
    <w-menu-item to="/git">Git</w-menu-item>
    <w-sub-menu to="advance">
      <template #title>进阶</template>
      <w-menu-item-group>
        <template #title>Vue工具库</template>
        <w-menu-item to="/vuex">Vuex</w-menu-item>
        <w-menu-item to="/vue-router">Vue Router</w-menu-item>
        <w-sub-menu to="unit-test">
          <template #title>单元测试</template>
          <w-menu-item to="/vue-test-utils">Vue Test Utils</w-menu-item>
          <w-menu-item to="/vue-testing-library">Vue Testing Library</w-menu-item>
        </w-sub-menu>
      </w-menu-item-group>
      <w-menu-item-group>
        <template #title>语言</template>
        <w-menu-item to="/typescript">TypeScript</w-menu-item>
        <w-menu-item to="/sass">Sass</w-menu-item>
      </w-menu-item-group>
    </w-sub-menu>
  </w-menu>
  <!-- Menu组件 end -->
  <div style="height: 50px;"></div>
  <!-- Notification组件 start -->
  <h3>• Notification通知</h3>
  <Button @click="change">Show Notification</Button>
  <Button @click="close">Close Notification</Button>
  <Notification ref=noti :ShowClose=true Title="Scuess" Content="Please continue" :Duration=duration :OffsetY=offsetY
    Type="scuess" Position="top-right" />
  <Notification :ShowClose=true Title="默认通知" Content="这是一个默认通知" :Duration=0 :OffsetY=150 Position="top-right" />
  <Notification Title="不可关闭" Content="此通知将会在10s后自动关闭" :Duration=10000 Position="bottom-right" :ShowClose=false />
  <!-- Notification组件 end -->

  <br />
  <!-- Pagination start -->
  <h3>• Pagination分页</h3>
  <Pagination />
  <!-- WVTable start -->
  <h3>• WVTable表格</h3>
  <WVTable />
  <!-- Input组件 start -->
  <h3>• Input输入框</h3>
  <p>(1) 基础样式</p>
  <w-input v-model="inputText1" placeholder="请输入..." />&emsp;输入的内容：{{ inputText1 }}
  <p>(2) 禁用状态 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;(3) 可清空</p>
  <w-input placeholder="请输入..." disabled /> &emsp;
  <w-input v-model="inputText2" placeholder="请输入..." clearable />{{ inputText2 }}
  <p>(4) 密码框 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;(5) 输入长度限制</p>
  <w-input v-model="inputText3" placeholder="请输入密码" show-password /> &emsp;
  <w-input v-model="inputText4" placeholder="请输入..." :maxlength="10" />
  <!-- Input组件 end -->
  <br />



  <!-- DateTimePicker组件 start -->
  <h3>• DateTimePicker日期选择器</h3>
  <DateTimePicker v-model="DateTimeValue" @update:model-value="updateDate" />
  <!-- DateTimePicker组件 end -->
  <br />

  <!-- Notification组件 start -->
  <h3>• Notification通知</h3>
  <button @click="change">Change</button>
  <button @click="close">Close</button>
  <Notification ref=noti :ShowClose=true Title="It is a error" Content="Please restart it" :Duration=duration
    :OffsetY=offsetY Type="error" Position="top-right" />
  <!-- Notification组件 end -->
  <br />

  <!-- Button组件 start -->
  <h3>• Button按钮</h3>
  <div class="basicUsage">
    <p>基础用法</p>
    <hr />
    <div class="row">
      <Button @click="btnClick">默认按钮</Button>
      <Button type="primary">主要按钮</Button>
      <Button type="success">成功按钮</Button>
      <Button type="info">信息按钮</Button>
      <Button type="warning">警告按钮</Button>
      <Button type="danger">危险按钮</Button>
    </div>
    <div class="row">
      <Button plain>默认按钮</Button>
      <Button plain type="primary">主要按钮</Button>
      <Button plain type="success">成功按钮</Button>
      <Button plain type="info">信息按钮</Button>
      <Button plain type="warning">警告按钮</Button>
      <Button plain type="danger">危险按钮</Button>
    </div>
    <div class="row">
      <Button round>默认按钮</Button>
      <Button round type="primary">主要按钮</Button>
      <Button round type="success">成功按钮</Button>
      <Button round type="info">信息按钮</Button>
      <Button round type="warning">警告按钮</Button>
      <Button round type="danger">危险按钮</Button>
    </div>
    <div class="row">
      <Button circle icon="search" />
      <Button circle type="primary" icon="edit" />
      <Button circle type="success" icon="check" />
      <Button circle type="info" icon="message" />
      <Button circle type="warning" icon="jinggao" />
      <Button circle type="danger" icon="delete" />
    </div>
  </div>
  <!-- 禁用状态 -->
  <div class="disabledState">
    <p>禁用状态</p>
    <hr />
    <div class="row">
      <Button disabled>默认按钮</Button>
      <Button disabled type="primary">主要按钮</Button>
      <Button disabled type="success">成功按钮</Button>
      <Button disabled type="info">信息按钮</Button>
      <Button disabled type="warning">警告按钮</Button>
      <Button disabled type="danger">危险按钮</Button>
    </div>
    <div class="row">
      <Button disabled plain>默认按钮</Button>
      <Button disabled plain type="primary">主要按钮</Button>
      <Button disabled plain type="success">成功按钮</Button>
      <Button disabled plain type="info">信息按钮</Button>
      <Button disabled plain type="warning">警告按钮</Button>
      <Button disabled plain type="danger">危险按钮</Button>
    </div>
  </div>
  <!-- 文字按钮 -->
  <div class="textBtn">
    <p>文字按钮</p>
    <hr />
    <div class="row">
      <Button type="text">文字按钮</Button>
      <Button type="text" disabled>文字按钮</Button>
    </div>
  </div>
  <!-- 加载中 -->
  <div class="loading">
    <p>加载中按钮</p>
    <hr />
    <div class="row">
      <Button disabled loading type="primary">加载中</Button>
    </div>
  </div>
  <!-- 不同尺寸 -->
  <div class="varisized">
    <p>不同尺寸</p>
    <hr />
    <div class="row">
      <Button>默认按钮</Button>
      <Button Size="medium">中等按钮</Button>
      <Button Size="small">小型按钮</Button>
      <Button Size="mini">超小按钮</Button>
    </div>
    <div class="row">
      <Button round>默认按钮</Button>
      <Button round Size="medium">中等按钮</Button>
      <Button round Size="small">小型按钮</Button>
      <Button round Size="mini">超小按钮</Button>
    </div>
  </div>
  <!-- Button组件 end -->
  <br />

  <!-- Layout组件 start -->
  <h3>• Layout布局</h3>
  <Row>
    <Col :span="24">
    <div class="grid-content bg-purple-dark"></div>
    </Col>
  </Row>
  <Row>
    <Col :span="12">
    <div class="grid-content bg-purple"></div>
    </Col>
    <Col :span="12">
    <div class="grid-content bg-purple-light"></div>
    </Col>
  </Row>
  <Row>
    <Col :span="8">
    <div class="grid-content bg-purple"></div>
    </Col>
    <Col :span="8">
    <div class="grid-content bg-purple-light"></div>
    </Col>
    <Col :span="8">
    <div class="grid-content bg-purple"></div>
    </Col>
  </Row>
  <Row>
    <Col :span="6">
    <div class="grid-content bg-purple"></div>
    </Col>
    <Col :span="6">
    <div class="grid-content bg-purple-light"></div>
    </Col>
    <Col :span="6">
    <div class="grid-content bg-purple"></div>
    </Col>
    <Col :span="6">
    <div class="grid-content bg-purple-light"></div>
    </Col>
  </Row>
  <Row>
    <Col :span="4">
    <div class="grid-content bg-purple"></div>
    </Col>
    <Col :span="4">
    <div class="grid-content bg-purple-light"></div>
    </Col>
    <Col :span="4">
    <div class="grid-content bg-purple"></div>
    </Col>
    <Col :span="4">
    <div class="grid-content bg-purple-light"></div>
    </Col>
    <Col :span="4">
    <div class="grid-content bg-purple"></div>
    </Col>
    <Col :span="4">
    <div class="grid-content bg-purple-light"></div>
    </Col>
  </Row>

  <!-- Layout组件 end -->


  <!-- WVcarousel组件 start -->
  <h3>• WVcarousel轮播图</h3>
  <WVcarousel :sliders="result"></WVcarousel>
  <!-- WVcarousel组件 end -->
  <!-- Icon组件 start -->
  <w-icon name="view" color="#000" size="30"></w-icon>
  <!-- Icon组件 end -->
</template>

<script setup lang="ts">
import Notification from '../../packages/components/notification/Notification.vue'
import DateTimePicker from '../../packages/components/DatePicker/DateTimePicker/DateTimePicker.vue'
import Button from '../../packages/components/Button/src/Button.vue'
import Row from '../../packages/components/Layout/src/row.vue'
import Col from '../../packages/components/Layout/src/col.vue'
import Pagination from '../../demo/Pagination/PagerDemo.vue'
import WVTable from '../../demo/Table/TableDemo.vue'
import '../../packages/fonts/iconfont.css'
import WVcarousel from '../../packages/components/carousel/WV-carousel.vue'

import {
  ref,
} from 'vue'

// WVcarousel轮播图
const result = [
  {
    "id": "16",
    "imgUrl": "http://yjy-xiaotuxian-dev.oss-cn-beijing.aliyuncs.com/picture/2021-04-15/dfc11bb0-4af5-4e9b-9458-99f615cc685a.jpg",
    "hrefUrl": "/category/1005000",
    "type": "1"
  },
  {
    "id": "19",
    "imgUrl": "http://yjy-xiaotuxian-dev.oss-cn-beijing.aliyuncs.com/picture/2021-04-15/6d202d8e-bb47-4f92-9523-f32ab65754f4.jpg",
    "hrefUrl": "/category/1013001",
    "type": "1"
  },
  {
    "id": "18",
    "imgUrl": "http://yjy-xiaotuxian-dev.oss-cn-beijing.aliyuncs.com/picture/2021-04-15/1ba86bcc-ae71-42a3-bc3e-37b662f7f07e.jpg",
    "hrefUrl": "/category/1013001",
    "type": "1"
  },
  {
    "id": "20",
    "imgUrl": "http://yjy-xiaotuxian-dev.oss-cn-beijing.aliyuncs.com/picture/2021-04-15/e83efb1b-309c-46f7-98a3-f1fefa694338.jpg",
    "hrefUrl": "/category/1005000",
    "type": "1"
  },
  {
    "id": "17",
    "imgUrl": "http://yjy-xiaotuxian-dev.oss-cn-beijing.aliyuncs.com/picture/2021-04-15/4a79180a-1a5a-4042-8a77-4db0b9c800a8.jpg",
    "hrefUrl": "/category/1019000",
    "type": "1"
  }
]
//  Notification组件
const duration = ref(3000)
const offsetY = ref(20)
const noti = ref<any>(null)
function change() {
  noti.value.IsVisible = true
}
function close() {
  noti.value.CloseNotification()
}

// Input组件
const inputText1 = ref('');
const inputText2 = ref('');
const inputText3 = ref('');
const inputText4 = ref('');

// DateTimePicker组件
let DateTimeValue = ref('123')
const updateDate = (val) => {
  DateTimeValue.value = val;
}

// button组件
function btnClick() {
  alert('触发按钮');
}

function handlerSelect(to) {
  console.log(to)
}
</script>

<style lang="scss" scoped>
.container {
  width: 700px;
  margin: 0 auto;
}

.row {
  margin-bottom: 20px;
}

.bg-purple-dark {
  background: #99a9bf;
}

.bg-purple {
  background: #d3dce6;
}

.bg-purple-light {
  background: #e5e9f2;
}

.grid-content {
  border-radius: 4px;
  min-height: 36px;
}

.flex-grow {
  flex-grow: 1;
}
</style>