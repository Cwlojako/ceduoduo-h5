<template>
  <div class="search">
    <div class="top">
      <img src="../../../assets/images/fanhui-1.png" alt="" class="fanhui" @click="$router.go(-1)">
      <van-search
        v-model="keyWord"
        shape="round"
        background="#FAFAFA"
        placeholder="请输入搜索关键词"
        @search="search"
      />
    </div>
    <div v-if="!hasSearch" style="text-align: center; font-size: 0.7rem; color: #999; margin-top: 1rem;">输入关键词进行搜索</div>
    <template v-else>
      <van-list
        v-model="loading"
        :finished="finished"
        finished-text="已经到底了~"
        @load="onLoad"
      >
        <div class="temp-wrap">
          <div class="temp-item" v-for="item in result" :key="item.id" @click="toTemplate(item.id)">
            <div class="temp-img"><img :src="item.cover ? item.cover : undefined" alt=""></div>
            <div class="temp-title">{{item.name}}</div>
          </div>
        </div>
      </van-list>
    </template>
  </div>
</template>

<script>
  import {searchTemplate} from '@/services/template'

  export default {
    name: "tempDetail",
    data() {
      return {
        keyWord: '',
        result: [],
        loading: false,
        finished: false,
        hasSearch: false,
        page: 0
      }
    },
    mounted() {

    },
    methods: {
      search() {
        this.result = [];
        this.page = 0;
        this.hasSearch = true;
        this.finished = false;
      },
      onLoad() {
        this.page += 1;
        let param =  {
          template: {
            name: this.keyWord
          },
          pageable: {
            page: this.page,
            size: 9,
            sort: {
              asc: ["orderNumber", "id"]
            }
          }
        };
        searchTemplate(param, res => {
          this.result = this.result.length === 0 ? this.result = res : this.result.concat(res);

          this.loading = false;
          if(res.length < 9) {
            this.finished = true;
          }
        });
      },
      toTemplate(tempId) {
        this.$router.push({
          name: 'tempDetail',
          query: {
            tempId: tempId
          }
        })
      }
    }
  }

</script>

<style lang="less" scoped>
  .search {
    width: 100%;
    height: 100vh;
    background-color: #FAFAFA;
    .top {
      border-bottom: 1px solid #E6E6E6;
      height: 2rem;
      line-height: 2rem;
      position: relative;
      .van-search {
        float: right;
        width: 90%;
        height: 100%;
        .van-search__content {
          background: #EEEEEE;
        }
      }
      .fanhui {
        position: absolute;
        left: 0.68rem;
        top: 0.6rem;
        width: 0.4rem;
        height: 0.78rem;
      }
    }
    .temp-wrap {
      text-align: center;
      display: flex;
      flex-flow: row wrap;
      justify-content: flex-start;
      .temp-item {
        flex: 0 0 33.33%;
        margin: 0.3rem 0 0.3rem;
        width: 33.33%;
        .temp-img {
          img {
            width: 4.2rem;
            height: 5.36rem;
            object-fit: contain;
          }
        }
        .temp-title {
          text-align: center;
          font-size:0.52rem;
          font-weight:bold;
          color:rgba(51,51,51,1);
        }
      }
    }
  }
</style>
