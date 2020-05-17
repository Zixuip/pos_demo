<template>
  <div>
    <el-row>
      <el-col :span="7" class="pos_order" id="order_list">
        <el-tabs>
          <el-tab-pane label="点餐">
            <el-table :data="tableData" border style="width:100%">
              <el-table-column prop="goodsName" label="商品名称" width="100"></el-table-column>
              <el-table-column prop="goodsAmount" label="数量" width="50"></el-table-column>
              <el-table-column prop="goodsPrice" label="价格" width="50"></el-table-column>
              <el-table-column label="操作" fixed="right">
                <template>
                  <el-button round type="primary" size="small">增加</el-button>
                  <el-button round type="danger" size="small">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <div class="btn">
              <el-button type="success">结账</el-button>
              <el-button type="warning">挂单</el-button>
              <el-button type="danger">删除</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <el-col :span="15">
        <div class="hot_goods">
          <div class="title">常用商品</div>
          <div class="hot_goods_list" v-for="items in hotGoods" :key="items.goodId">
            <ul>
              <li>
                <span>{{items.goodsName}}</span>
                <span class="hot_price">￥{{items.goodsPrice}}元</span>
              </li>
            </ul>
          </div>
        </div>

        <div class="goods_type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="foodList">
                  <li v-for="(items,index) in typeGoods" :key="index">
                    <span class="foodImg">
                      <img :src="items.foodImg" width="100%" />
                    </span>
                    <span class="foodName">{{items.foodName}}</span>
                    <span class="foodPrice">￥{{items.foodPrice}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">饮料</el-tab-pane>
            <el-tab-pane label="小食">小食</el-tab-pane>
            <el-tab-pane label="套餐">套餐</el-tab-pane>
          </el-tabs>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: "pos",
  data() {
    return {
      tableData: [
        {
          goodsName: "香辣鸡腿堡",
          goodsAmount: 1,
          goodsPrice: 18
        },
        {
          goodsName: "可口可乐（大杯）",
          goodsAmount: 1,
          goodsPrice: 8
        }
      ],
      hotGoods: [
        {
          goodsId: 1,
          goodsName: "香辣鸡腿堡",
          goodsPrice: 18
        },
        {
          goodsId: 2,
          goodsName: "可口可乐（大杯）",
          goodsPrice: 8
        },
        {
          goodsId: 3,
          goodsName: "板烧鸡腿堡",
          goodsPrice: 15
        }
      ],
      typeGoods: [
        {
          foodImg: "../../static/labao.jpg",
          foodName: "香辣鸡腿堡",
          foodPrice: 18.0
        },
        {
          foodImg: "../../static/banshao.jpg",
          foodName: "板烧鸡腿堡",
          foodPrice: 18.0
        },
        {
          foodImg: "../../static/cola_m.jpg",
          foodName: "可口可乐(中)",
          foodPrice: 8.0
        }
      ]
    };
  },
  methods: {
    order_list() {
      /* 
        读取订单栏的高度，并赋值给ta的样式高度，形成动态高度
      */
      let orderHeight = document.body.clientHeight;
      console.log(orderHeight);
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
  font-size: 14px;
}
.btn {
  margin-top: 10px;
  margin-left: 10px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 9.5px;
  text-align: left;
  font-size: 14px;
}
.hot_goods_list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 10px;
  background-color: #fff;
}
.hot_price {
  color: #58b7ff;
}

.goods_type {
  /* 清除浮动 */
  clear: both;
}

.foodList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auto;
  overflow: hidden;
  background-color: #fff;
  padding-top: 2px;
  float: left;
  margin: 2px;
}
.foodList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
  height: 70px;
}
.foodName {
  font-size: 18px;
  padding-left: 10px;
  color: brown;
}
.foodPrice {
  font-size: 16px;
  padding-left: 10px;
  padding-top: 10px;
}
</style>