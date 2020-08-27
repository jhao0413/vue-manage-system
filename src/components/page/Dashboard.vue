<template>
  <!-- 首页组件 -->
  <div>
    <!-- 行布局容器 gutter属性为栅格间隔 -->
    <el-row :gutter="20">
      <!-- 列布局容器 Element UI 将一行分为 24列，通过 span来表示占据的列数的份额-->
      <el-col :span="8">
        <!-- 卡片布局容器 shadow：设置阴影显示时机-->
        <!-- 用户登录信息卡片栏 -->
        <el-card shadow="hover" class="mgb20" style="height:252px;">
          <div class="user-info">
            <!-- 用户头像 -->
            <img src="../../assets/img/own.jpg" class="user-avator" alt="" />
            <div class="user-info-cont">
              <!-- 用户名 -->
              <div class="user-info-name">{{name}}</div>
              <!-- 用户身份 -->
              <div>{{role}}</div>
            </div>
          </div>
          <div class="user-info-list">
            上次登录时间：<span>2018-01-01</span>
          </div>
          <div class="user-info-list">上次登录地点：<span>东莞</span></div>
        </el-card>
        <!-- Card 组件包括header和body部分，header部分需要有显式具名 slot 分发，同时也是可选的。 -->
        <!-- 编程语言卡片 -->
        <el-card shadow="hover" style="height:252px;">
          <div slot="header" class="clearfix">
            <span>语言详情</span>
          </div>
          Vue
          <!-- 进度条 percentage：进度百分比 -->
          <el-progress :percentage="71.3" color="#42b983"></el-progress>
          JavaScript
          <el-progress :percentage="24.1" color="#f1e05a"></el-progress>
          CSS
          <el-progress :percentage="3.7"></el-progress>
          HTML
          <el-progress :percentage="0.9" color="#f56c6c"></el-progress>
        </el-card>
      </el-col>
      <!-- 网站信息卡片；占据 16/24 的列份额 -->
      <el-col :span="16">
        <!-- 在已划分的列宽中再次分为 24 份，通过 el-row 和 el-col 进行划分 -->
        <el-row :gutter="20" class="mgb20">
          <el-col :span="8">
            <!-- 用户访问量卡片 -->
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-1">
                <i class="el-icon-lx-people grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">1234</div>
                  <div>用户访问量</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <!-- 系统信息卡片 -->
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-2">
                <i class="el-icon-lx-notice grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">321</div>
                  <div>系统消息</div>
                </div>
              </div>
            </el-card>
          </el-col>
          <!-- 数量卡片 -->
          <el-col :span="8">
            <el-card shadow="hover" :body-style="{padding: '0px'}">
              <div class="grid-content grid-con-3">
                <i class="el-icon-lx-goods grid-con-icon"></i>
                <div class="grid-cont-right">
                  <div class="grid-num">5000</div>
                  <div>数量</div>
                </div>
              </div>
            </el-card>
          </el-col>
        </el-row>
        <!-- 待办事项卡片 -->
        <el-card shadow="hover" style="height:403px;">
          <div slot="header" class="clearfix">
            <span>待办事项</span>
            <el-button style="float: right; padding: 3px 0" type="text"
              >添加</el-button
            >
          </div>
          <el-table
            :data="todoList"
            :show-header="false"
            height="304"
            style="width: 100%;font-size:14px;"
          >
            <!-- 当el-table元素中注入data对象数组后，在el-table-column中用prop属性来对应对象中的键名即可填入数据，用label属性来定义表格的列名。可以使用width属性来定义列宽。 -->
            <el-table-column width="40">
              <!--  <template slot-scope="scope"> -->
              <!-- slot-scope：作用域插槽，已经废弃。现在使用v-slot代替，用于将对象里面的值取出来放置在插槽位置-->
              <template v-slot="scope">
                <!-- 这个插槽设置复选框的初始状态 -->
                <el-checkbox v-model="scope.row.status"></el-checkbox>
              </template>
            </el-table-column>
            <el-table-column>
              <template v-slot="scope">
                <!-- 这个插槽放置data中的title属性值 -->
                <div
                  class="todo-item"
                  :class="{'todo-item-del': scope.row.status}"
                >
                  {{scope.row.title}}
                </div>
              </template>
            </el-table-column>
            <el-table-column width="60">
              <template v-slot="scope">
                <i class="el-icon-edit"></i>
                <i class="el-icon-delete"></i>
              </template>
            </el-table-column>
          </el-table>
        </el-card>
      </el-col>
    </el-row>
    <!-- <el-row :gutter="20">
      <el-col :span="12">
        图表1
        <el-card shadow="hover">
          canvasId：canvas标签的id，必填。
                type：图表类型，必填。
                options：图表配置参数，可选，具体参考文档。
                data：要生成图表的数据
          <schart
            ref="bar"
            class="schart"
            canvasId="bar"
            :data="data"
            type="bar"
            :options="options"
          ></schart>
        </el-card>
      </el-col>
      <el-col :span="12">
        图表2
        <el-card shadow="hover">
          <schart
            ref="line"
            class="schart"
            canvasId="line"
            :data="data"
            type="line"
            :options="options2"
          ></schart>
        </el-card>
      </el-col>
    </el-row> -->
  </div>
</template>

<script>
  import Schart from "vue-schart";
  import bus from "../../utils/bus";
  export default {
    name: "dashboard",
    data() {
      return {
        // 从本地存储获取用户名
        name: localStorage.getItem("ms_username"),
        // 待办事项卡片模拟数据
        todoList: [
          {
            title: "今天要修复100个bug",
            status: false,
          },
          {
            title: "今天要修复100个bug",
            status: false,
          },
          {
            title: "今天要写100行代码加几个bug吧",
            status: false,
          },
          {
            title: "今天要修复100个bug",
            status: false,
          },
          {
            title: "今天要修复100个bug",
            status: true,
          },
          {
            title: "今天要写100行代码加几个bug吧",
            status: true,
          },
        ],
        // 图表公用模拟数据
        data: [
          {
            name: "2018/09/04",
            value: 1083,
          },
          {
            name: "2018/09/05",
            value: 941,
          },
          {
            name: "2018/09/06",
            value: 1139,
          },
          {
            name: "2018/09/07",
            value: 816,
          },
          {
            name: "2018/09/08",
            value: 327,
          },
          {
            name: "2018/09/09",
            value: 228,
          },
          {
            name: "2018/09/10",
            value: 1065,
          },
        ],
        // 图表一设置
        options: {
          title: "最近七天每天的用户访问量",
          showValue: false,
          fillColor: "rgb(45, 140, 240)",
          bottomPadding: 30,
          topPadding: 30,
        },
        // 图表二设置
        options2: {
          title: "最近七天用户访问趋势",
          fillColor: "#FC6FA1",
          axisColor: "#008ACD",
          contentColor: "#EEEEEE",
          bgColor: "#F5F8FD",
          bottomPadding: 30,
          topPadding: 30,
        },
      };
    },
    components: {
      Schart,
    },
    computed: {
      role() {
        // 利用三元表达式对用户登录后身份进行判定
        return this.name === "AsunaCC" ? "超级管理员" : "普通用户";
      },
    },
    // 在创建vue对象时，当html渲染之前就触发，只会触发一次；
    created() {
      this.handleListener();
      this.changeDate();
    },
    // activated,deactivated这两个生命周期函数一定是要在使用了keep-alive组件后才会有的
    // 在vue对象存活的情况下，进入当前存在activated()函数的页面时，一进入页面就触发
    activated() {
      this.handleListener();
    },
    // 退出当前存在deactivated()函数页面时触发
    deactivated() {
      // 清除事件监听
      window.removeEventListener("resize", this.renderChart);
      // 清除监听
      bus.$off("collapse", this.handleBus);
    },
    methods: {
      changeDate() {
        // 获取 1970 年 1 月 1 日至今的毫秒数。
        const now = new Date().getTime();
        this.data.forEach((item, index) => {
          // 将data中的模拟数据转化为当前日期为基准的时间
          const date = new Date(now - (6 - index) * 86400000);
          // 将毫秒数转化为对应的日期时间并赋予给data.name，getMonth返回的是0~11所以要加1
          item.name = `${date.getFullYear()}/${
            date.getMonth() + 1
          }/${date.getDate()}`;
        });
      },
      handleListener() {
        // 通过bus监听事件，在接受数据后调用handleBus方法
        bus.$on("collapse", this.handleBus);
        // 通过事件监听，判定浏览器尺寸，调用renderChart方法对图表进行重新渲染
        window.addEventListener("resize", this.renderChart);
      },
      handleBus(msg) {
        setTimeout(() => {
          this.renderChart();
        }, 300);
      },
      renderChart() {
        // bar 图表重新渲染
        this.$refs.bar.renderChart();
        // line 图表重新渲染
        this.$refs.line.renderChart();
      },
    },
  };
</script>

<style scoped>
  .el-row {
    margin-bottom: 20px;
  }

  .grid-content {
    display: flex;
    align-items: center;
    height: 100px;
  }

  .grid-cont-right {
    flex: 1;
    text-align: center;
    font-size: 14px;
    color: #999;
  }

  .grid-num {
    font-size: 30px;
    font-weight: bold;
  }

  .grid-con-icon {
    font-size: 50px;
    width: 100px;
    height: 100px;
    text-align: center;
    line-height: 100px;
    color: #fff;
  }

  .grid-con-1 .grid-con-icon {
    background: rgb(45, 140, 240);
  }

  .grid-con-1 .grid-num {
    color: rgb(45, 140, 240);
  }

  .grid-con-2 .grid-con-icon {
    background: rgb(100, 213, 114);
  }

  .grid-con-2 .grid-num {
    color: rgb(45, 140, 240);
  }

  .grid-con-3 .grid-con-icon {
    background: rgb(242, 94, 67);
  }

  .grid-con-3 .grid-num {
    color: rgb(242, 94, 67);
  }

  .user-info {
    display: flex;
    align-items: center;
    padding-bottom: 20px;
    border-bottom: 2px solid #ccc;
    margin-bottom: 20px;
  }

  .user-avator {
    width: 120px;
    height: 120px;
    border-radius: 50%;
  }

  .user-info-cont {
    padding-left: 50px;
    flex: 1;
    font-size: 14px;
    color: #999;
  }

  .user-info-cont div:first-child {
    font-size: 30px;
    color: #222;
  }

  .user-info-list {
    font-size: 14px;
    color: #999;
    line-height: 25px;
  }

  .user-info-list span {
    margin-left: 70px;
  }

  .mgb20 {
    margin-bottom: 20px;
  }

  .todo-item {
    font-size: 14px;
  }

  .todo-item-del {
    text-decoration: line-through;
    color: #999;
  }

  .schart {
    width: 100%;
    height: 300px;
  }
</style>