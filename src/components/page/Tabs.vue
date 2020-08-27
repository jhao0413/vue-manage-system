<template>
  <!-- tab选项卡组件 -->
  <div class="">
    <div class="crumbs">
      <!-- 面包屑导航 separator 表示分隔符 -->
      <el-breadcrumb separator="/">
        <el-breadcrumb-item
          ><i class="el-icon-lx-copy"></i> tab选项卡</el-breadcrumb-item
        >
      </el-breadcrumb>
    </div>
    <div class="container">
      <!-- 标签页：分割内容上有关联，但属于不同类别的数据集合。v-model/value 绑定值，用于选中选项卡中的 name -->
      <el-tabs v-model="message">
        <!-- 选项卡1 绑定 label 实时统计未读消息数量 -->
        <el-tab-pane :label="`未读消息(${unread.length})`" name="first">
          <!-- 绑定 data 获取 unread 数组中的对象。 绑定 show-header 来动态改变表头的显示状态-->
          <el-table :data="unread" :show-header="false" style="width: 100%">
            <el-table-column>
              <!-- 通过 v-sort 动态分发数据 -->
              <template v-slot="scope">
                <span class="message-title">{{scope.row.title}}</span>
              </template>
            </el-table-column>
            <!-- el-table 上绑定了 data 对象数组。通过 prop 键入对象中的键名即可填入数据 -->
            <el-table-column prop="date" width="180"></el-table-column>
            <el-table-column width="120">
              <template v-slot="scope">
                <el-button size="small" @click="handleRead(scope.$index)"
                  >标为已读</el-button
                >
              </template>
            </el-table-column>
          </el-table>
          <div class="handle-row">
            <!-- 通过 type 改变风格 -->
            <el-button type="primary" @click="handleAllRead"
              >全部标为已读</el-button
            >
          </div>
        </el-tab-pane>

        <!-- 绑定 label 实时统计已读消息数量 -->
        <el-tab-pane :label="`已读消息(${read.length})`" name="second">
          <!-- 对是否选中当前标签页进行判断 -->
          <template v-if="message === 'second'">
            <el-table :data="read" :show-header="false" style="width: 100%">
              <el-table-column>
                <!-- 动态分发数据 -->
                <template v-slot="scope">
                  <span class="message-title">{{scope.row.title}}</span>
                </template>
              </el-table-column>
              <!-- 动态渲染data中模拟的时间 -->
              <el-table-column prop="date" width="150"></el-table-column>
              <el-table-column width="120">
                <template v-slot="scope">
                  <el-button type="danger" @click="handleDel(scope.$index)"
                    >删除</el-button
                  >
                </template>
              </el-table-column>
            </el-table>
            <div class="handle-row">
              <!-- 通过 type 改变样式 danger 为危险按钮样式 -->
              <el-button type="danger" @click="handleAllDel"
                >删除全部</el-button
              >
            </div>
          </template>
        </el-tab-pane>
        <!-- 绑定 label 实时统计回收站消息数量 -->
        <el-tab-pane :label="`回收站(${recycle.length})`" name="third">
          <!-- 对是否选中当前标签页进行判断 -->
          <template v-if="message === 'third'">
            <el-table :data="recycle" :show-header="false" style="width: 100%">
              <el-table-column>
                <!-- 动态分发数据 -->
                <template v-slot="scope">
                  <span class="message-title">{{scope.row.title}}</span>
                </template>
              </el-table-column>
              <el-table-column prop="date" width="150"></el-table-column>
              <el-table-column width="120">
                <template v-slot="scope">
                  <el-button @click="handleRestore(scope.$index)"
                    >还原</el-button
                  >
                </template>
              </el-table-column>
            </el-table>
            <div class="handle-row">
              <!-- 通过 type 改变样式 danger 为危险按钮样式 -->
              <el-button type="danger" @click="handleAllDelete"
                >清空回收站</el-button
              >
            </div>
          </template>
        </el-tab-pane>
      </el-tabs>
    </div>
  </div>
</template>

<script>
  import bus from "../../utils/bus";

  export default {
    name: "tabs",
    data() {
      return {
        message: "first",
        showHeader: false,
        unread: [
          {
            date: "2018-04-19 20:00:00",
            title: "【系统通知】该系统将于今晚凌晨2点到5点进行升级维护",
          },
          {
            date: "2018-04-19 21:00:00",
            title: "今晚12点整发大红包，先到先得",
          },
        ],
        read: [
          {
            date: "2018-04-19 19:00:00",
            title: "【系统通知】该系统将于今晚凌晨2点到5点进行升级维护",
          },
        ],
        recycle: [
          {
            date: "2018-04-19 18:00:00",
            title: "【系统通知】该系统将于今晚凌晨2点到5点进行升级维护",
          },
        ],
      };
    },
    methods: {
      // 操作对应信息，标为已读，获取传入的当前信息索引
      handleRead(index) {
        // 将未读信息对象数组当前索引上的对象删除，并返回给 item
        const item = this.unread.splice(index, 1);
        // 将 item 和 存放已读信息的对象数组进行拼接。
        this.read = item.concat(this.read);
        // 通过 bus 通信将当前未读信息的数量进行传递
        bus.$emit("msg", this.unread.length);
      },
      // 操作所有信息，标为已读
      handleAllRead() {
        // 将未读信息对象数组中所有信息全部删除，并返回给 item
        const item = this.unread.splice(0, this.unread.length);
        // 将 item 和 存放已读信息的对象数组进行拼接。
        this.read = item.concat(this.read);
        // 通过 bus 通信将当前未读信息的数量进行传递
        bus.$emit("msg", this.unread.length);
      },
      // 操作对应信息，标为回收，获取传入的当前信息索引
      handleDel(index) {
        // 将已读信息对象数组当前索引上的对象删除，并返回给 item
        const item = this.read.splice(index, 1);
        // 将 item 和 存放回收信息的对象数组进行拼接。
        this.recycle = item.concat(this.recycle);
      },
      // 操作所有信息，标为回收
      handleAllDel() {
        // 将已读信息对象数组中所有信息全部删除，并返回给 item
        const item = this.read.splice(0, this.read.length);
        // 将 item 和 存放回收信息的对象数组进行拼接。
        this.recycle = item.concat(this.recycle);
      },
      // 操作对应信息，标为已读，获取传入的当前信息索引
      handleRestore(index) {
        // 将回收信息对象数组当前索引上的对象删除，并返回给 item
        const item = this.recycle.splice(index, 1);
        // 将 item 和 存放已读信息的对象数组进行拼接。
        this.read = item.concat(this.read);
      },
      // 操作所有信息，彻底删除
      handleAllDelete() {
        // 将回收信息对象数组中所有信息全部删除
        this.recycle.splice(0, this.recycle.length);
      },
    },
    computed: {
      unreadNum() {
        return this.unread.length;
      },
    },
  };
</script>

<style>
  .message-title {
    cursor: pointer;
  }
  .handle-row {
    margin-top: 30px;
  }
</style>