<template>
  <div class="wrapper">
    <hr />
    <br />
    <button @click="send">send</button>
  </div>
</template>
<script>
  import Bus from "../../utils/bus";
  export default {
    data() {
      return {
        message: "",
      };
    },
    methods: {
        // 创建 Bus 实现非父子组件通信

        //将数据传递到Second组件中
      send() {
        this.message = "这是传递到 second 中的数据!";
        Bus.$emit("msg", this.message);
      },
    },
  };
</script>