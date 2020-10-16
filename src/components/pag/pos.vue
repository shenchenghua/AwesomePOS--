<template>
  <div class="pos">
    <el-row>
      <el-card>
        <el-col :span="8" class="pos-order" >
          <el-tabs>
            <el-tab-pane label="点餐">
              <el-table :data="tableData" border style="width: 100%">
                <el-table-column
                  prop="goodsName"
                  label="商品名称"
                ></el-table-column>
                <el-table-column
                  prop="count"
                  label="数量"
                  width="50"
                ></el-table-column>
                <el-table-column
                  prop="price"
                  label="金额"
                  width="70"
                ></el-table-column>
                <el-table-column label="操作" width="100" fixed="right">
                  <template scope="scope">
                    <el-button
                      type="text"
                      size="small"
                      @click="delSingleGoods(scope.row)"
                      >删除</el-button
                    >
                    <el-button
                      type="text"
                      size="small"
                      @click="addOrderList(scope.row)"
                      >增加</el-button
                    >
                  </template>
                </el-table-column>
              </el-table>
              <div class="totalDiv">
                <small>数量：</small>
                {{ totalCount }} &nbsp;&nbsp;&nbsp;
                <small>金额：</small>
                {{ totalMoney }}元
              </div>
              <div class="div-btn" style="text-align: center">
                <el-button type="warning">挂单</el-button>
                <el-button type="danger" @click="delAllGoods()">删除</el-button>
                <el-button type="success" @click="checkout()">结账</el-button>
                <br>
              </div>
            </el-tab-pane>
            <el-tab-pane label="挂单">挂单</el-tab-pane>
            <el-tab-pane label="外卖">外卖</el-tab-pane>
          </el-tabs>
        </el-col>
        <el-col :span="15" :offset="1">
          <div class="often-goods">
            <div class="title">常用商品</div>
            <div class="often-goods-list">
              <ul style="cursor: pointer">
                <li
                  v-for="goods in oftenGoods"
                  @click="addOrderList(goods)"
                  :key="goods.goodsName"
                >
                  <span>{{ goods.goodsName }}</span>
                  <span class="o-price">￥{{ goods.price }}</span>
                </li>
              </ul>
            </div>
          </div>
          <div class="goods-type">
            <el-tabs>
              <el-tab-pane label="汉堡">
                <ul class="cookList">
                  <li
                    v-for="goods in type0Goods"
                    :key="goods.goodsImg"
                    @click="addOrderList(goods)"
                  >
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="小食">
                <ul class="cookList">
                  <li
                    v-for="goods in type1Goods"
                    :key="goods.goodsImg"
                    @click="addOrderList(goods)"
                  >
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="饮料">
                <ul class="cookList">
                  <li
                    v-for="goods in type2Goods"
                    :key="goods.goodsImg"
                    @click="addOrderList(goods)"
                  >
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>
              <el-tab-pane label="套餐">
                <ul class="cookList">
                  <li
                    v-for="goods in type3Goods"
                    :key="goods.goodsImg"
                    @click="addOrderList(goods)"
                  >
                    <span class="foodImg">
                      <img :src="goods.goodsImg" width="100%" />
                    </span>
                    <span class="foodName">{{ goods.goodsName }}</span>
                    <span class="foodPrice">￥{{ goods.price }}元</span>
                  </li>
                </ul>
              </el-tab-pane>
            </el-tabs>
          </div>
        </el-col>
      </el-card>
    </el-row>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "pos",
  data() {
    return {
      tableData: [],
      oftenGoods: [
        {
          goodsId: 1,
          goodsName: "香辣鸡腿堡",
          price: 18,
        },
        {
          goodsId: 2,
          goodsName: "田园鸡腿堡",
          price: 15,
        },
        {
          goodsId: 3,
          goodsName: "和风汉堡",
          price: 15,
        },
        {
          goodsId: 4,
          goodsName: "快乐全家桶",
          price: 80,
        },
        {
          goodsId: 5,
          goodsName: "脆皮炸鸡腿",
          price: 10,
        },
        {
          goodsId: 6,
          goodsName: "魔法鸡块",
          price: 20,
        },
        {
          goodsId: 7,
          goodsName: "可乐大杯",
          price: 10,
        },
        {
          goodsId: 8,
          goodsName: "雪顶咖啡",
          price: 18,
        },
        {
          goodsId: 9,
          goodsName: "大块鸡米花",
          price: 15,
        },
        {
          goodsId: 20,
          goodsName: "香脆鸡柳",
          price: 17,
        },
      ],
      type0Goods: [
        // {
        //   goodsId: 1,
        //   goodsImg:"https://p1.meituan.net/xianfu/2a95aa5e6f0d507741faa76c387fb21739936.jpg@130w_130h_1e_1c",
        //   goodsName: "香辣鸡腿堡",
        //   price: 18
        // },
        // {
        //   goodsId: 2,
        //   goodsImg:
        //     "https://p1.meituan.net/xianfu/1dd1fc665f0f4da6c3284735dcf1d13334816.jpg@130w_130h_1e_1c",
        //   goodsName: "田园鸡腿堡",
        //   price: 15
        // },
        // {
        //   goodsId: 3,
        //   goodsImg:
        //     "https://p1.meituan.net/xianfu/a732f6934b725a9679e43637d2b1b30736864.jpg@130w_130h_1e_1c",
        //   goodsName: "和风汉堡",
        //   price: 15
        // },
        // {
        //   goodsId: 4,
        //   goodsImg:
        //     "https://p0.meituan.net/xianfu/276c6b2d6417bd90a322cb451ee914d858368.jpg@130w_130h_1e_1c",
        //   goodsName: "快乐全家桶",
        //   price: 80
        // },
        // {
        //   goodsId: 5,
        //   goodsImg:
        //     "https://p0.meituan.net/xianfu/a62f2cae0f163020d92bac6ddd2c4f9961440.jpg@130w_130h_1e_1c",
        //   goodsName: "脆皮炸鸡腿",
        //   price: 10
        // },
        // {
        //   goodsId: 6,
        //   goodsImg:
        //     "https://p0.meituan.net/xianfu/c299fdf37904126a1362285b7cae667f48128.jpg@130w_130h_1e_1c",
        //   goodsName: "魔法鸡块",
        //   price: 20
        // },
        // {
        //   goodsId: 7,
        //   goodsImg:
        //     "https://p0.meituan.net/xianfu/5a54ba8d4b03144bcb7d0711ef6f125662464.jpg@130w_130h_1e_1c",
        //   goodsName: "可乐大杯",
        //   price: 10
        // },
        // {
        //   goodsId: 8,
        //   goodsImg:
        //     "https://dss0.bdstatic.com/70cFuHSh_Q1YnxGkpoWK1HF6hhy/it/u=2477490940,3228705952&fm=115&gp=0.jpg",
        //   goodsName: "雪顶咖啡",
        //   price: 18
        // },
        // {
        //   goodsId: 9,
        //   goodsImg:
        //     "https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3716174910,4095383137&fm=26&gp=0.jpg",
        //   goodsName: "大块鸡米花",
        //   price: 15
        // },
        // {
        //   goodsId: 20,
        //   goodsImg:
        //     "https://p0.meituan.net/xianfu/0c00e7cae7c9268c3efb700394043b3137888.jpg@130w_130h_1e_1c",
        //   goodsName: "香脆鸡柳",
        //   price: 17
        // }
      ],
      type1Goods: [],
      type2Goods: [],
      type3Goods: [],
      // totalMoney: 0,
      // totalCount: 0,
    };
  },
  created() {
    axios
      .get(
        "https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/oftenGoods"
      )
      .then((response) => {
        //  console.log(response);
        this.oftenGoods = response.data.oftenGoods;
      })
      .catch((error) => {
        console.log(error);
        alert("网络错误，不能访问");
      });

    axios
      .get(
        "https://www.fastmock.site/mock/0bf6a5bae7eab8507e44b56191ddff36/vuepos/typeGoods"
      )
      .then((response) => {
        //  console.log(response);
        this.type0Goods = response.data.data[0];
        this.type1Goods = response.data.data[1];
        this.type2Goods = response.data.data[2];
        this.type3Goods = response.data.data[3];
      })
      .catch((error) => {
        console.log(error);
        alert("网络错误，不能访问");
      });
  },

  methods: {
    addOrderList(goods) {
      //判断商品是否存在列表中
      let isHave = false;
      for (let i = 0; i < this.tableData.length; i++) {
        //  console.log(this.tableData[i].goodsId);
        if (this.tableData[i].goodsId == goods.goodsId) {
          isHave = true;
        }
      }
      //根据判断写逻辑业务
      if (isHave) {
        //改变列表中商品数量
        function check(o) {
          return o.goodsId == goods.goodsId;
        }
        let arr = this.tableData.filter(check);
        arr[0].count++;
        //  console.log(arr);
      } else {
        //不存在就推入数组
        let newGoods = {
          goodsId: goods.goodsId,
          goodsName: goods.goodsName,
          price: goods.price,
          count: 1,
        };
        this.tableData.push(newGoods);
      }
      this.getAllMoney();
    },
    //删除单个商品
    delSingleGoods(goods) {
      console.log(goods);
      this.tableData = this.tableData.filter((o) => o.goodsId != goods.goodsId);
      this.getAllMoney();
    },
    //删除所有商品
    delAllGoods() {
      this.tableData = [];
      this.totalCount = 0;
      this.totalMoney = 0;
    },
    //结账
    checkout() {
      if (this.totalCount != 0) {
        this.tableData = [];
        this.totalCount = 0;
        this.totalMoney = 0;
        this.$message({
          message: "结账成功，感谢你又为店里出了一份力!",
          type: "success",
        });
      } else {
        this.$message.error("不能空结。老板了解你急切的心情！");
      }
    },
    //汇总数量和金额
    // getAllMoney() {
    //   this.totalCount = 0;
    //   this.totalMoney = 0;
    //   if (this.tableData) {
    //     //进行数量和价格的汇总计算
    //     // console.table(this.tableData)

    //     this.tableData.forEach((element) => {
    //       this.totalCount += element.count;
    //       this.totalMoney = this.totalMoney + element.price * element.count;
    //     });
    //   }
    // },
  },
  computed:{
    totalCount(){
      let _count = 0
      if (this.tableData) {
        this.tableData.forEach((element) => {
          _count += element.count;
        });
      }
      return _count
    },
     totalMoney(){
        let _money = 0
        if (this.tableData) {
        this.tableData.forEach((element) => {
          _money = _money + element.price * element.count;
        });
        return _money
      }
     },
    c_total(){
      
    }
  }
};
</script>

<style scoped>
.pos-order {
  background-color: #f9fafc;
  border-right: 1px solid #c0ccda;
}
.div-btn {
  margin-top: 10px;
}
.title {
  height: 20px;
  border-bottom: 1px solid #d3dce6;
  background-color: #f9fafc;
  padding: 10px;
}
.often-goods-list ul li {
  list-style: none;
  float: left;
  border: 1px solid #e5e9f2;
  padding: 10px;
  margin: 5px;
  background-color: #fff;
}
.o-price {
  color: #58b7ff;
}
.goods-type {
  clear: both;
}
.cookList li {
  list-style: none;
  width: 23%;
  border: 1px solid #e5e9f2;
  height: auot;
  overflow: hidden;
  background-color: #fff;
  padding: 2px;
  float: left;
  margin: 2px;
  cursor: pointer;
}
.cookList li span {
  display: block;
  float: left;
}
.foodImg {
  width: 40%;
  height: 100%;
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
.totalDiv {
  background-color: #fff;
  padding: 10px;
  border: 1px solid #d3dce6;
  text-align: center;
}
</style>