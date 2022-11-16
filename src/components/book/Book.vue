<template>
  <div ref="books" class="book">
    <vheader :back="true" :search="true"></vheader>

    <ul class="mui-table-view">
      <li class="mui-table-view-cell mui-media">
        <a href="javascript:;" class>
          <img class="mui-media-object mui-pull-left" :src="books.cover" />
          <div class="mui-media-body">
            {{ title }}
            <p class="mui-ellipsis">
              <span>邻家小姐</span>
              <span class="span2">9999</span>
              <span>9999999999字</span>
            </p>
            <p>
              <span>已完结</span>
              <span>{{ books.minorCateV2 }}</span>
            </p>
          </div>
        </a>
        <!-- 加入书架阅读 -->
        <yuedu :booklinks="books._id"></yuedu>
      </li>
    </ul>

    <ul class="mui-table-view mui-grid-view">
      <li class="mui-table-view-cell mui-media mui-col-xs-4">
        <div class="mui-media-body">
          <p>追人气</p>
        </div>
        <div class="mui-media-body">
          <p class="body-p">99999</p>
        </div>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4">
        <div class="mui-media-body">
          <p>读者留存率</p>
        </div>
        <div class="mui-media-body">
          <p class="body-p">100%</p>
        </div>
      </li>
      <li class="mui-table-view-cell mui-media mui-col-xs-4">
        <div class="mui-media-body">
          <p>日更新字数/天</p>
        </div>
        <div class="mui-media-body">
          <p class="body-p">9999999</p>
        </div>
      </li>
    </ul>

  </div>
</template>
<script>
import { mapState, mapMutations } from "vuex";
import { books } from "../api/api.js";
import { Indicator } from "mint-ui";
import { BOOK_PAGE } from "../store/mutationsType.js";
import { formatDate } from "../time/time.js";
import yuedu from "../sub/yuedu"; // 加入书架
import pinglun from "../sub/pinglun"; // 评论
import recommend from "../sub/recommend"; // 评论
import vheader from "../sub/header";

export default {
  components: {
    yuedu,
    pinglun,
    recommend,
    vheader,
  },
  data() {
    return {
      books: {},
      title: '',
      flag: true,
    };
  },
  created() {
    // this.getlist();
    this.title = this.$route.params.name
  },

  computed: {
    // 处理字数过万的
    wordCount() {
      return this.books.wordCount > 10000
        ? parseInt(this.books.wordCount / 10000) + "万"
        : this.books.wordCount;
    },
    // 处理人气过千的
    latelyFollower() {
      return this.books.latelyFollower > 1000
        ? parseInt(this.books.latelyFollower / 1000) + "k"
        : this.books.latelyFollower;
    },
  },
  filters: {
    formatDate(time) {
      var date = new Date(time);
      return formatDate(date, "yyyy-MM-dd hh:mm");
    },
  },
  methods: {
    ...mapMutations(["SET_HEADER_INFO", "SET_BOOK"]),
    getlist() {
      Indicator.open();
      books(this.$route.params.id).then((res) => {
        if (res.status === 200) {
          Indicator.close();
          this.books = this.imgUrl(res.data);
          this.SET_BOOK(this.books); //书籍信息存在session 详情页获取书籍id 获取对应的源-目录-内容
        }
      });
    },
    //图片解密 跟去掉http的前面字符串
    imgUrl(arr) {
      arr.cover = unescape(arr.cover);
      arr.cover = arr.cover.replace("/agent/", "");
      return arr;
    },
    // 更多介绍
    more() {
      if (this.flag) {
        this.$refs.pl.className = "";
        this.$refs.span.className = "mui-icon mui-icon-arrowup";
        this.flag = false;
      } else {
        this.$refs.pl.className = "pop";
        this.$refs.span.className = "mui-icon mui-icon-arrowdown";
        this.flag = true;
      }
    },
    topshow() {
      this.$refs.books.scrollTop = 0;
    },
  },
};
</script>
<style lang="scss" scoped>
.book {
  position: relative;
  top: 0;
  .mui-table-view {
    .mui-media-body {
      font-size: 14px;
    }
    img {
      max-width: 64px;
      height: 100px;
    }
    .span2 {
      border-right: 1px solid #666;
      border-left: 1px solid #666;
      padding: 0px 5px;
    }
    span {
      font-size: 12px;
      color: #666;
      margin-right: 5px;
    }
  }
  .body-p {
    color: #333;
  }
  .long {
    padding: 10px;
    p {
      color: #333;
      font-size: 12px;
    }
    .pop {
      display: -webkit-box;
      -webkit-box-orient: vertical;
      -webkit-line-clamp: 3;
      overflow: hidden;
    }
    span {
      margin-top: -25px;
      display: flex;
      justify-content: flex-end;
    }
  }
  .mulu {
    padding: 10px;
    border-top: 1px solid #ebebeb;
    border-bottom: 1px solid #ebebeb;
    display: flex;
    justify-content: space-between;
    font-size: 12px;
    :nth-child(1) {
      font-size: 14px;
      color: #333;
    }
    :nth-child(2) {
      padding: 0px 10px;
    }
  }
}
</style>
