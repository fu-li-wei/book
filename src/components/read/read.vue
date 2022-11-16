<template>
  <div class="read" ref="dvtop">
    <div class="btn">
      <mt-button type="default" size="small" @click="backGo()" plain>返回</mt-button>
    </div>

    <div style="min-height: 500px; padding-top: 30px">
      <h1 class="read-h1">{{ name }}</h1>
      <div class="read-text" style="padding:10px; color: #000" v-html="content"></div>
    </div>
  </div>
</template>
<script>

export default {
  data() {
    return {
      con: [],
      getBook: {},
      iss: "0",
      booklinkss: [],
      booktitle: [],

      id: this.$route.params.id,
      link: this.$route.params.link,
      bookhylist: {},
      showMulu: false,

      name: '',
      content: "",
    };
  },
  created() {
    this.name = this.$route.params.name
  },
  mounted() {
    //  更新数据market_id.txt文件接口
    let xhr = null, okStatus = document.location.protocol === "file:" ? 0 : 200;
    xhr = window.XMLHttpRequest ? new XMLHttpRequest() : new ActiveXObject('Microsoft.XMLHTTP')      
    xhr.open("GET", `/text/${this.name}`, false); // 文件路径
    xhr.overrideMimeType("text/html;charset=utf-8"); //默认为utf-8
    xhr.send(null);
    console.log(xhr.status === okStatus ? xhr.responseText : null); //文本的内容
    let text =  xhr.status === okStatus ? xhr.responseText : null
    text = text.replaceAll(' ','<p>')
    this.content = text
  },
  methods: {
    backGo() {
      this.$router.push({ name: "book" });
    },
  },
};
</script>
<style lang="scss" scoped>
.read {
  overflow-y: auto;
  z-index: 101;
  position: absolute;
  top: 0;
  width: 100%;
  height: 100%;
  background-color: #c4b395;
  padding: 20px;
  line-height: 24px;
  letter-spacing: 2px;
  .read-h1 {
    font-size: 20px;
  }
  h1 {
    font-size: 16px;
    color: #222;
  }
  .read-text {
    p {
      color: #000 !important;
    }
    text-indent: 2em;
  }
  .btn {
    display: flex;
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    height: 50px;
    background: #c4b395;
    padding-top: 10px;
    justify-content: center;
    button {
      margin: 0 10px;
      font-size: 12px;
      line-height: 1.2;
      height: 28px;
      color: #425faf;
      border-color: #666;
      &.mint-button--default {
        color: #333;
      }
    }
  }
}
</style>
