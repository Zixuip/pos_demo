<template>
  <div>
    <el-row>
      <el-col :span="7" class="pos_order" id="order_list">我是订单栏</el-col>
      <el-col :span="7">我是产品栏</el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "pos",
  data() {
    return {};
  },
  methods: {
    order_list() {
      /* 
        读取订单栏的高度，并赋值给ta的样式高度，形成动态高度
      */
      let orderHeight = document.body.clientHeight;
      document.getElementById("order_list").style.height = orderHeight + "px";
    }
  },
  mounted() {
    this.order_list();
  }
};
</script>

<style>
.pos_order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
</style>