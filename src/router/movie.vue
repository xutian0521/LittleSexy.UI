<template>
  <div id="el-movle">
    <el-card  shadow="hover">
      <el-row>
      <el-col style="padding-bottom: 3%;margin-left: 0%; margin-right: 3%; width:30%" :span="8" v-for="(item, index) in movieList" :key="index" :offset="index > 0 ? 2 : 0">
          <el-card :body-style="{ padding: '10px' ,height: cardHeight+'px'}"  shadow="always">
            <img :src="item.cover" class="image">
            <div style="padding: 14px;">
              <h4 style="padding-bottom: 10px;height:22px;line-height:16px;overflow: hidden;white-space: nowrap;text-overflow: ellipsis;">{{item.title}}</h4>
              <span >番号：{{item.fanHao}}</span>
              <div class="bottom clearfix">
                <time class="time">{{item.date}}</time>
                <a :href="item.linkUrl">
                  <router-link :to="{path:'movie/detail', query:{ id: item.id}}">观看</router-link>
                </a>
              </div>
            </div>
          </el-card>
        </el-col>
      </el-row>
    </el-card>
    <div class="block">
      <el-pagination layout="prev, pager, next" :total="1000"></el-pagination>
    </div>
  </div>
</template>
<script lang="ts">
import Vue from "vue";
import ElementUI from "element-ui";
import "element-ui/lib/theme-chalk/index.css";
import axios from "axios";
import common from "../common.js";

export default {
  name: "el-movie",
  data() {
    return {
      movieList: [],
      total: "0",
      currentPage: 4,
      cardHeight: 500,
    };
  },
        watch:{
            'screenWidth':function(val){ //监听屏幕宽度变化
                

            },
            'screenHeight':function(){ //监听屏幕高度变化

            }
        },
  mounted() {
    this.init();
  },
  methods: {
    init: function() {
      let _self = this;

      axios
        .get(`${common.apiurl}/api/Movie/List`)
        .then(function(response) {
          var apiData = response.data.content;
          _self.$data.movieList = apiData;
          //debugger;
          //console.log(_self.$data.movieList);
        })
        .catch(function(error) {
          console.log(error);
        });
        // console.log((1920/937) /0.51)=4.01;
        let  _h = (4.0178291167054*0.51 * (window.innerWidth)) * 0.3 /1.158683798520758 /2;
        this.cardHeight = (window.innerWidth/ 3)/1.158683798520758;
        console.log(_h);
        
        console.log((window.innerWidth/ 3)/1.158683798520758);
        
    },
    handleSizeChange(val) {
      console.log(`每页 ${val} 条`);
    },
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
    }
  }
};
</script>


<style>
.time {
  font-size: 13px;
  color: #999;
}

.bottom {
  margin-top: 13px;
  line-height: 12px;
}

.button {
  padding: 0;
  float: right;
}

.image {
  width: 100%;
  display: block;
}

.clearfix:before,
.clearfix:after {
  display: table;
  content: "";
}

.clearfix:after {
  clear: both;
}
</style>

