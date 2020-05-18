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
                <template scope="scope">
                  <el-button round type="primary" size="small" @click="addOrderList(scope.row)">增加</el-button>
                  <el-button round type="danger" size="small" @click="del(scope.row)">删除</el-button>
                </template>
              </el-table-column>
            </el-table>
            <!-- 金额结算 -->
            <div class="total">
              <small>数量：{{totalNum}}</small>
              <br />
              <mid>总价：{{totalMoney}}￥</mid>
            </div>
            <!-- 按钮 -->
            <div class="btn">
              <el-button type="success" @click="chekout">结账</el-button>
              <el-button type="warning">挂单</el-button>
              <el-button type="danger" @click="delAll">删除</el-button>
            </div>
          </el-tab-pane>
          <el-tab-pane label="挂单">挂单</el-tab-pane>
          <el-tab-pane label="外卖">外卖</el-tab-pane>
        </el-tabs>
      </el-col>
      <!-- 热销商品 -->
      <el-col :span="15">
        <div class="hot_goods">
          <div class="title">常用商品</div>
          <div class="hot_goods_list">
            <ul>
              <li v-for="(items,goodsId) in hotGoods" :key="goodsId" @click="addOrderList(items)">
                <span>{{items.goodsName}}</span>
                <span class="hot_price">￥{{items.price}}元</span>
              </li>
            </ul>
          </div>
        </div>
        <!-- 汉堡类 -->
        <div class="goods_type">
          <el-tabs>
            <el-tab-pane label="汉堡">
              <div>
                <ul class="foodList">
                  <li v-for="(items,index) in type0Goods" :key="index" @click="addOrderList(items)">
                    <span class="foodImg">
                      <img :src="items.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="饮料">
              <div>
                <ul class="foodList">
                  <li v-for="(items,index) in type1Goods" :key="index" @click="addOrderList(items)">
                    <span class="foodImg">
                      <img :src="items.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="小食">
              <div>
                <ul class="foodList">
                  <li v-for="(items,index) in type2Goods" :key="index" @click="addOrderList(items)">
                    <span class="foodImg">
                      <img :src="items.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
            <el-tab-pane label="套餐">
              <div>
                <ul class="foodList">
                  <li v-for="(items,index) in type3Goods" :key="index" @click="addOrderList(items)">
                    <span class="foodImg">
                      <img :src="items.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{items.goodsName}}</span>
                    <span class="foodPrice">￥{{items.price}}元</span>
                  </li>
                </ul>
              </div>
            </el-tab-pane>
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
      tableData: [],
      hotGoods: [],
      type0Goods: [],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      goods: [],
      totalMoney: 0,
      totalNum: 0
    };
  },
  methods: {
    order_list() {
      /* 
        读取订单栏的高度，并赋值给ta的样式高度，形成动态高度
      */
      let orderHeight = document.body.clientHeight;
      // console.log(orderHeight);
      document.getElementById("order_list").style.height = orderHeight + "px";
    },
    addOrderList(goods) {
      this.totalMoney = 0;
      this.totalNum = 0;
      // 判断商品是否已经存在订单列表
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      // 根据判断的值再写业务逻辑
      if (isHave) {
        // 改变列表中商品数量
        let arr = this.tableData.filter(item => item.goodsId == goods.goodsId);
        arr[0].goodsAmount++;
      } else {
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          goodsPrice: goods.price,
          goodsAmount: 1
        };
        this.tableData.push(newGoods);
      }
      this.getAllMoney();
    },
    /* 删除单个商品 */
    del(goods) {
      this.tableData = this.tableData.filter(
        item => item.goodsId != goods.goodsId
      );
      this.getAllMoney();
    },
    // 重新计算金额
    getAllMoney() {
      this.totalNum = 0;
      this.totalMoney = 0;
      if (this.tableData) {
        /* 计算价格 */
        this.tableData.forEach(element => {
          this.totalNum += element.goodsAmount;
          this.totalMoney =
            this.totalMoney + element.goodsPrice * element.goodsAmount;
        });
      }
    },
    // 清空订单商品
    delAll() {
      this.tableData = [];
      this.totalMoney = 0;
      this.totalNum = 0;
    },
    // 模拟结账
    chekout() {
      if (this.totalNum != 0) {
        this.delAll();
        this.$message({
          message: "结算成功",
          type: "success"
        });
      } else {
        this.$message.error("订单为空");
      }
    }
  },
  /*  computed: {
    totalCount() {
      return this.tableData.reduce((pre, goods) => {
        return pre + goods.goodsAmount;
      }, 0);
    },
    totalPrice() {
      return this.tableData.reduce((pre, goods) => {
        return pre + goods.goodsAmount * goods.price;
      }, 0);
    }
  }, */
  mounted() {
    this.order_list();
  },
  created() {
    this.$http
      .get(
        "https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/oftenGoods"
      )
      .then(res => {
        // console.log(res);
        this.hotGoods = res.data.oftenGoods;
      });
    this.$http
      .get(
        "https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/typeGoods"
      )
      .then(res => {
        // console.log(res);
        this.type0Goods = res.data.data[0];
        this.type1Goods = res.data.data[1];
        this.type2Goods = res.data.data[2];
        this.type3Goods = res.data.data[3];
      })
      .catch(error => {
        alert("请求失败，请刷新网络");
      });
  }
};
</script>

<style>
.pos {
  font-size: 12px;
}
.pos_order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
  font-size: 14px;
}
.btn {
  margin-top: 10px;
  text-align: center;
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
  margin: 5px;
  background-color: #fff;
  cursor: pointer;
}
.hot_price {
  color: #58b7ff;
}

.goods_type {
  /* 清除浮动 */
  clear: both;
  cursor: pointer;
}
.hot_goods_list {
  border-bottom: 1px solid #c0ccda;
  height: auto;
  overflow: hidden;
  padding-bottom: 10px;
  background-color: #f9fafc;
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
  height: 75px;
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
.total {
  height: auto;
  overflow: hidden;
  text-align: center;
  font-size: 16px;
  background-color: #fff;
  border-bottom: 1px solid #e5e9f2;
  padding: 10px;
}
</style>