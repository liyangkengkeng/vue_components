<template>
  <div>
    <div class="components-cat" style="z-index:106;">
      <router-view></router-view>
      <ul class="components-list">
        <li @click="operate(cat.value)" v-for="(cat, index) in componentsLeve1List" :key="index" :class="cat.progress ? cat.progress : ''" v-tooltip="cat.name">
          {{ cat.name }}
          <template v-if="cat.progress && cat.progress==='improve'">
            <span class="completion-flag">(待完善)</span>
          </template>
          <template v-if="cat.progress && cat.progress==='done'">
            <span class="completion-flag">(完成)</span>
          </template>
          <template v-if="cat.progress && cat.progress==='notStart'">
            <span class="completion-flag">(未开始)</span>
          </template>
        </li>
      </ul>
    </div>
    <modal :show="showModal" @close='closeModal' @ok='okModal'>
      <div slot="content">
        <p>模态框</p>
        <div class="mt-20">
          <button class="btn line1-btn" @click='showSecondModal = true'>打开第二个模态框</button>
        </div>
      </div>
    </modal>
    <modal :show="showSecondModal" @close='closeSecondModal' @ok='okSecodeModal' :small=true>
      <div slot="content">
        <p>我是第二个模态框</p>
      </div>
    </modal>
    <modal :show="showFileUploadModal" @close='closeFileUploadModal'>
      <div slot="content">
        <file-upload></file-upload>
      </div>
    </modal>

    <!-- dropzone 文件上传 可拖拽-->
    <modal :show="showDropzoneModal" @close='showDropzoneModal = false'>
      <div slot="content">
        <p></p>
        <Dropzone></Dropzone>
      </div>
    </modal>
    <modal :show="showSelectionModal" @close='showSelectionModal = false'>
      <div slot="content">
        <selection :multi='true' v-model="selected" :select-list='selectList'></selection>
        <selection :multi='true' v-model="selected1" :select-list='selectList1' :value="'value'" :text="'text'"></selection>
        <div style="margin-top:190px;">
          <span style="margin-right:30px;">选择的水果: {{ selected }}</span>
          <span>选择的课程: {{ selected1 }}</span>
        </div>
      </div>
    </modal>
    <modal :show="showLoadingModal" @close='showLoadingModal = false'>
      <div slot="content" v-loading.lg='loading'>
        <table style="width:100%;height:100px;border:1px solid #eee;padding:10px;">
          <thead>
            <tr>
              <th>col1</th>
              <th>col2</th>
              <th>col3</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>test1</td>
              <td>test2</td>
              <td>test3</td>
            </tr>
          </tbody>
        </table>
      </div>
    </modal>
    <!--
                                  地图-->

    <modal :show="showMapModal" @close='showMapModal = false' :large=true>
      <div slot="content">
        <BaiduMap></BaiduMap>
      </div>
    </modal>

    <!--
                                  treeMenu树形菜单-->

    <modal :show="showTreeMenuModal" @close='showTreeMenuModal = false' :large=true>
      <div slot="content">
        <div>树形菜单</div>
        <TreeMenu :nodes="treeData" @select-menu="selectMenu"></TreeMenu>
      </div>
    </modal>

    <!--
                                  面包屑导航-->

    <modal :show="showBreadCrumbModal" @close='showBreadCrumbModal = false' :large=true>
      <div slot="content">
        <div>面包屑导航</div>
        <div class="content">
          <div class="side-menu" style="float:left;width:30%;height:200px;">
            <router-link to="/news" class="menu-level level-1">
              News
            </router-link>
            <router-link to="/news/news1" class="menu-level level-2">
              News1
            </router-link>
            <router-link to="/news/news1/content1" class="menu-level level-3">
              content1
            </router-link>
            <router-link to="/news/news1/content2" class="menu-level level-3">
              content2
            </router-link>                                      
            <router-link to="/news/news2" class="menu-level level-2">
              News2
            </router-link> 
            <router-link to="/news/news2/content" class="menu-level level-3">
              content
            </router-link>                       
            <router-link to="/home" class="menu-level level-1">
              Home
            </router-link>
          </div>
          <div class="right-content" style="float:left;width:70%;height:200px;">
            <router-view>
            </router-view>
            <div>

            </div>
          </div>
        </div>
      </div>
    </modal>

    <!--
                                  轮播图-->

    <modal :show="showSwiperModal" @close='showSwiperModal = false' :large=true>
      <div slot="content">
        <div>轮播图</div>
        <swiper :img-list="imgList" :auto-play="true"></swiper>
      </div>
    </modal>

    <!-- 多选框 -->
    <modal :show="showCheckBoxModal" @close="showCheckBoxModal = false">
      <div slot="content">
        <div>多选框</div>
        <Checkbox :list='checkboxList' :selected.sync="selectedCheckboxList"></Checkbox>
        <div>
          <div v-for='(item, index) in selectedCheckboxList' :key="index">
            <input type='text' v-model=item.content />
          </div>
        </div>
      </div>
    </modal>
  </div>
</template>

<style scoped>
.components-cat {
  margin: 30px 20px;
  z-index: 105;
}

.components-cat .components-list {
  display: flex;
  flex-direction: column;
  width: 100%;
  flex-wrap: wrap;
  max-height: 460px;
  height: 460px;
}

.components-cat .components-list li {
  height: 40px;
  width: 200px;
  line-height: 40px;
  background: #8ec2dc;
  border-bottom: 1px solid #eee;
  list-style: none;
}

.components-cat .components-list li:hover {
  background: #eee;
  cursor: pointer;
}


/*组件完成标志*/

.done {
  color: #309e34;
}


/*组件未开始写标志*/

.notStart {
  color: #7e79a5;
}


/*组件开始写仍需要完善标志*/

.improve {
  color: #f5850e;
}

.completion-flag {
  font-size: 10px;
  color: #585050;
}

.side-menu .menu-level {
  display: block;
  height: 30px;
  width: 180px;
  line-height: 30px;
  background: #eee;
  border: 1px solid #d1cece;
  margin-bottom: 1px;
  text-align: left;
}

.side-menu .level-1 {
  padding-left: 20px;
}

.side-menu .level-2 {
  width: 160px;
  padding-left: 40px;
}

.side-menu .level-3 {
  width: 140px;
  padding-left: 60px;
}
</style>

<script>
import Modal from '@/components/modal';
import FileUpload from '@/components/FileUpload';
import Selection from '@/components/Select';
import Checkbox from '@/components/Checkbox';
import BaiduMap from '@/components/map';
import TreeMenu from '@/components/VueTree';
import Swiper from '@/components/Swiper';
import Dropzone from '@/components/Dropzone';
import img1 from '@/assets/4.jpg';

export default {
  name: 'ComponentsCat',
  components: {
    Modal,
    FileUpload,
    Selection,
    BaiduMap,
    TreeMenu,
    Swiper,
    Checkbox,
    Dropzone,
  },
  data() {
    return {
      showModal: false,
      showSecondModal: false,
      showFileUploadModal: false,
      showSelectionModal: false,
      showLoadingModal: false,
      showMapModal: false,
      showTreeMenuModal: false,
      showSwiperModal: false,
      showCheckBoxModal: false,
      showDropzoneModal: false,
      showBreadCrumbModal: false,
      loading: false,
      selected: '',
      selected1: '',
      selectList: ['苹果', '香蕉', '橘子', '菠萝', '西瓜'], // 最简单的一种传值
      selectList1: [
        {
          text: '数学',
          value: 'math',
        },
        {
          text: '英语',
          value: 'english',
        },
        {
          text: '语文',
          value: 'chinese',
        },
        {
          text: '物理',
          value: 'physics',
        },
        {
          text: '化学',
          value: 'chemical',
        },
        {
          text: '历史',
          value: 'history',
        },
      ],
      checkboxList: [
        {
          text: '姓名',
          value: 'name',
          content: '姓名',
        },
        {
          text: '手机',
          value: 'tel',
          content: '手机',
        },
        {
          text: '公司',
          value: 'company',
          content: '公司',
        },
      ],
      selectedCheckboxList: [],
      componentsLeve1List: [
        {
          name: '模态框',
          value: 'modal',
          progress: 'improve',
        },
        {
          name: '弹出框',
          value: 'alert',
          progress: 'done',
        },
        {
          name: '下拉框',
          value: 'selection',
          progress: 'improve',
        },
        {
          name: '面包屑导航',
          value: 'breadCrumb',
          progress: 'improve',
        },
        {
          name: '分页',
          value: 'pagination',
          progress: 'notStart',
        },
        {
          name: '加载中',
          value: 'loading',
          progress: 'done',
        },
        {
          name: '树形菜单',
          value: 'tree',
          progress: 'improve',
        },
        {
          name: 'Tab页',
          value: 'tab',
          progress: 'improve',
        },
        {
          name: '日期组件',
          value: 'datePicker',
          progress: 'notStart',
        },
        {
          name: '表格',
          value: 'table',
          progress: 'notStart',
        },
        {
          name: '单选按钮',
          value: 'radio',
          progress: 'notStart',
        },
        {
          name: '多选框',
          value: 'checkbox',
          progress: 'improve',
        },
        {
          name: '级联选择器',
          value: 'cascader',
          progress: 'notStart',
        },
        {
          name: '滑块',
          value: 'slider',
          progress: 'notStart',
        },
        {
          name: '上传',
          value: 'upload',
          progress: 'improve',
        },
        {
          name: '评分',
          value: 'rate',
          progress: 'notStart',
        },
        {
          name: '消息提示',
          value: 'message',
          progress: 'notStart',
        },
        {
          name: '导航菜单',
          value: 'navMenu',
          progress: 'notStart',
        },
        {
          name: '步骤条',
          value: 'steps',
          progress: 'notStart',
        },
        {
          name: '滚动条',
          value: 'scroll',
          progress: 'notStart',
        },
        {
          name: '开关',
          value: 'swicth',
          progress: 'notStart',
        },
        {
          name: '折叠面板',
          value: 'collapse',
          progress: 'notStart',
        },
        {
          name: '时间轴',
          value: 'timeLine',
          progress: 'notStart',
        },
        {
          name: '可收缩侧边栏',
          value: 'aside',
          progress: 'notStart',
        },
        {
          name: 'tooltip气泡提示',
          value: 'tooltip',
          progress: 'improve',
        },
        {
          name: '复制粘贴',
          value: 'clipboard',
          progress: 'notStart',
        },
        {
          name: '进度条',
          value: 'progress',
          progress: 'notStart',
        },
        {
          name: '拖拽',
          value: 'drag',
          progress: 'notStart',
        },
        {
          name: '百度地图',
          value: 'map',
          progress: 'improve',
        },
        {
          name: '穿梭框',
          value: 'transfer',
          progress: 'notStart',
        },
        {
          name: '轮播图',
          value: 'swiper',
          progress: 'improve',
        },
        {
          name: 'dropzone上传',
          value: 'dropzone',
          progress: 'improve',
        },
      ],
      treeData: [
        {
          text: 'row1',
          value: 'row1',
          children: [
            {
              text: 'row1-1',
              value: 'row1-1',
              children: [
                {
                  text: 'row1-1-1',
                  value: 'row1-1-1',
                  children: [
                    {
                      text: 'row1-1-1-1',
                      value: 'row1-1-1-1',
                    },
                    {
                      text: 'row1-1-1-2',
                      value: 'row1-1-1-2',
                    },
                  ],
                },
                {
                  text: 'row1-1-2',
                  value: 'row1-1-2',
                  children: [
                    {
                      text: 'row1-1-2-1',
                      value: 'row1-1-2-1',
                    },
                    {
                      text: 'row1-1-2-2',
                      value: 'row1-1-2-2',
                    },
                  ],
                },
              ],
            },
            {
              text: 'row1-2',
              value: 'row1-2',
            },
          ],
        },
        {
          text: 'row2',
          value: 'row2',
          children: [
            {
              text: 'row2-1',
              value: 'row2-1',
            },
          ],
        },
      ],
      imgList: [ // vue引入图片的方式 http://www.jb51.net/article/119782.htm
        '../static/imgs/1.jpg',
        img1,
        '../static/imgs/2.jpg',
        '../static/imgs/3.jpg',
      ],
    };
  },
  watch: {
    '$route'(to, from) {
      console.log(this.$route)
    }
  },
  mounted() {
    // 测试一个带参数的Vuex getter写法
    // console.log(this.$store.getters.test(1));
  },
  methods: {
    clickShowModalBtn() {
      this.showModal = true;
    },
    closeModal() {
      this.showModal = false;
    },
    closeSecondModal() {
      this.showSecondModal = false;
    },
    closeFileUploadModal() {
      this.showFileUploadModal = false;
    },
    okModal() {

    },
    okSecodeModal() {
      this.$alert('test', '弹出的消息', 'alert', () => {

      });
    },
    selectFruit(e) {
      console.log(e.target.value);
    },
    selectMenu(node) {
      console.log(node);
    },
    operate(type) {
      switch (type) {
        case 'modal':
          this.clickShowModalBtn();
          break;
        case 'alert':
          this.showAlert();
          break;
        case 'upload':
          this.showFileUploadModal = true;
          break;
        case 'selection':
          this.showSelectionModal = true;
          break;
        case 'loading':
          this.showLoadingModal = true;
          this.loading = true; // 关闭模态框并没有销毁模态框组件
          setTimeout(() => {
            this.loading = false;
          }, 5000);
          break;
        case 'map':
          this.showMapModal = true;
          break;
        case 'tree':
          this.showTreeMenuModal = true;
          break;
        case 'swiper':
          this.showSwiperModal = true;
          break;
        case 'checkbox':
          this.showCheckBoxModal = true;
          break;
        case 'dropzone':
          this.showDropzoneModal = true;
          break;
        case 'breadCrumb':
          this.showBreadCrumbModal = true;
          break;
        default:
      }
    },
    showAlert() {
      this.$alert('test', '弹出的消息', 'alert', () => {

      });
    },
  },
};
</script>

    