<template>
  <div class="sss">
    <div class="qh">
      <el-tabs v-model="activeName">
        <el-tab-pane label="点餐" name="first">
          <el-table :data="tableData" style="width: 100%">
            <el-table-column prop="goodsName" label="商品名称" width="160">
            </el-table-column>
            <el-table-column prop="price" label="价格" width="100">
            </el-table-column>
            <el-table-column prop="count" label="数量"> </el-table-column>
            <el-table-column label="操作">
              <template slot-scope="scope">
                <el-button type="text" @click="romovelist(scope)"
                  >删除</el-button
                >
                <el-button type="text" @click="dianji(scope.row)"
                  >增加</el-button
                >
              </template>
            </el-table-column>
          </el-table>
          <div>
            <span>合计：</span>
            <span>{{ allmoney }}</span>
            <span>总数量：</span>
            <span>{{ allcount }}</span>
          </div>
          <el-button type="warning" @click="guadan">挂单</el-button>
          <el-button type="danger" @click="empty">清空</el-button>
          <el-button type="primary" @click="checkout">结账</el-button>
        </el-tab-pane>
        <el-tab-pane label="挂单" name="second">
          <el-table :data="outerlist" border style="width: 100%">
              <el-table-column prop="datetime" label="挂单时间" width="120">
              </el-table-column>
              <el-table-column label="操作" width="472">
                <template slot-scope="scope">
                  <el-button type="text" @click="diancan(scope)">继续点餐</el-button>
                  <el-button type="text" @click="qingkong1(scope)"
                    >删除</el-button>
                    <el-popover placement="right" width="400" trigger="click">
                      <el-table :data="scope.row.caozuo">
                        <el-table-column
                          width="150"
                          property="goodsName"
                          label="商品名称"
                        ></el-table-column>
                        <el-table-column
                          width="100"
                          property="price"
                          label="价格"
                        ></el-table-column>
                        <el-table-column
                          width="300"
                          property="count"
                          label="数量"
                        ></el-table-column>
                      </el-table>
                     <el-button slot="reference">查看详情</el-button>
                    </el-popover>
               
                </template>
                 </el-table-column>
            </el-table>
        </el-tab-pane>
        <el-tab-pane label="外卖" name="third">角色管理</el-tab-pane>
      </el-tabs>
    </div>
    <div>
      <div><p>常用商品</p></div>
      <div class="shangpo">
        <div
          class="shangp"
          v-for="itme in ouderlist"
          :key="itme.id"
          @click="dianji(itme)"
        >
          <div>
            {{ itme.goodsName }} <span>￥{{ itme.price }}</span>
          </div>
        </div>
      </div>
      <hr />
      <div>
        <el-tabs v-model="activeNames" type="card">
          <el-tab-pane label="汉堡" name="first">
            <div class="saa">
              <div
                class="ass"
                v-for="itme in ouderlist1"
                :key="itme.id"
                @click="dianji(itme)"
              >
                <div>
                  <img :src="itme.goodsImg" alt="" class="s" />
                </div>
                <div>
                  <p>{{ itme.goodsName }}</p>
                  <p>￥{{ itme.price }}</p>
                </div>
              </div>
            </div>
          </el-tab-pane>
          <el-tab-pane label="小食" name="second">
            <div class="saa">
              <div
                class="ass"
                v-for="itme in ouderlist2"
                :key="itme.id"
                @click="dianji(itme)"
              >
                <div>
                  <img :src="itme.goodsImg" alt="" class="s" />
                </div>
                <div>
                  <p>{{ itme.goodsName }}</p>
                  <p>￥{{ itme.price }}</p>
                </div>
              </div>
            </div>
          </el-tab-pane>
          <el-tab-pane label="饮品" name="third">
            <div class="saa">
              <div
                class="ass"
                v-for="itme in ouderlist3"
                :key="itme.id"
                @click="dianji(itme)"
              >
                <div>
                  <img :src="itme.goodsImg" alt="" class="s" />
                </div>
                <div>
                  <p>{{ itme.goodsName }}</p>
                  <p>￥{{ itme.price }}</p>
                </div>
              </div>
            </div>
          </el-tab-pane>
          <el-tab-pane label="套餐" name="fourth">
            <div class="saa">
              <div
                class="ass"
                v-for="itme in ouderlist4"
                :key="itme.id"
                @click="dianji(itme)"
              >
                <div>
                  <img :src="itme.goodsImg" alt="" class="s" />
                </div>
                <div>
                  <p>{{ itme.goodsName }}</p>
                  <p>￥{{ itme.price }}</p>
                </div>
              </div>
            </div>
          </el-tab-pane>
        </el-tabs>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      msg: "sssss",
      activeName: "first",
      activeNames: "first",
      tableData: [],
      ouderlist: [],
      ouderlist1: [],
      ouderlist2: [],
      ouderlist3: [],
      ouderlist4: [],
      outerlist:[]
    };
  },
  methods: {
    romovelist(data) {
      if (data.row.count > 1) {
        data.row.count--;
        this.$set(this.tableData, data.$index, data.row);
      } else {
        this.tableData.splice(data.$index, 1);
      }
    },
    // addlist(data) {
    //   data.row.count++;
    // },
    dianji(data) {
      var flag = false;
      this.tableData.forEach((e, i) => {
        if (e.goodsName == data.goodsName) {
          flag = true;
          e.count++;
          this.$set(this.tableData, i, e);
        }
      });
      if (!flag) {
        data.count = 1;
        this.tableData.push(data);
      }
    },
    empty() {
      this.tableData = [];
    },
     guadan() {
       if(this.tableData.length!=0){
         var date = new Date();
         var h = date.getHours();
         var m = date.getMinutes();
         var s = date.getSeconds();
         this.outerlist.push({
           datetime: h + ":" + m + ":" + s,
           caozuo: this.tableData,
         });
         this.empty();
       }else{

       }
      
    },
    diancan(order) {
      console.log(order);
      this.tableData = order.row.caozuo;
      this.qingkong1(order);
    },
    qingkong1(order) {
      this.outerlist.splice(order.$index, 1);
    },
     checkout() { 
      if (this.allcount != 0) {
        this.tableData = [];
        this.$message({
          message: "结账成功，感谢你又为店里出了一份力!",
          type: "success",
        });
      } else {
          this.$message.error("不能空结。老板了解你急切的心情!");
        }
       
    },
  },
  computed: {
    allmoney: function () {
      var allaa = 0;
      this.tableData.forEach((element) => {
        allaa += element.count * element.price;
      });
      return allaa;
    },
    allcount: function () {
      var allbb = 0;
      this.tableData.forEach((element) => {
        allbb += element.count;
      });
      return allbb;
    },
  },
  created() {
    axios.get("http://192.168.1.196:8081/getOffenGoods").then((response) => {
     
      response.data.resultInfo.forEach((e) => {
        e.price = Number(e.price);
        this.ouderlist.push(e);
      });
    });
    axios.get("http://192.168.1.196:8081/getAllGoods").then((response) => {
      response.data.resultInfo.forEach((e) => {
        if (e.goodType == "汉堡") {
          this.ouderlist1.push(JSON.parse(e.goods));
        } else if (e.goodType == "小食") {
          this.ouderlist2.push(JSON.parse(e.goods));
        } else if (e.goodType == "饮品") {
          this.ouderlist3.push(JSON.parse(e.goods));
        } else {
          this.ouderlist4.push(JSON.parse(e.goods));
        }
      });
      // console.log(response);
    });
  },
};
</script>
<style  >
.qh {
  width: 450px;
  margin-left: 10px;
}
.sss {
  display: flex;
}
.shangp {
  width: 200px;
  height: 40px;
  background-color: cornsilk;
  text-align: center;
  line-height: 40px;
  margin: 5px;
}
.shangpo {
  width: 1000px;
  display: flex;
  flex-wrap: wrap;
}
.ass {
  width: 200px;
  height: 100px;
  background-color: cornsilk;
  display: flex;
  text-align: center;
  margin: 10px;
}
.saa {
  width: 1000px;
  display: flex;
  flex-wrap: wrap;
}
.s {
  width: 80px;
  height: 60px;
  margin-top: 10px;
}
</style>