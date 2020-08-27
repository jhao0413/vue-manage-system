<template>
  <div class="wrapper">
    <hr />
    <br />
    <p>{{message}}</p>
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
    mounted() {
        //接收来自First组件的信息
      let self = this;
      Bus.$on("msg", (e) => {
        self.message = e;
      });
    },
  };
</script>