<template>
  <div class="clearfix selector">
    <div class="type-wrap logo">
      <div class="fl key brand">品牌</div>
      <div class="value logos">
        <!-- 品牌地方 -->
        <ul class="logo-list">
          <li
            v-for="(trademark, index) in trademarkList"
            :key="trademark.tmId"
            @click="tradeMarkHandler(trademark)"
          >
            {{ trademark.tmName }}
          </li>
        </ul>
      </div>
      <div class="ext">
        <a href="javascript:void(0);" class="sui-btn">多选</a>
        <a href="javascript:void(0);">更多</a>
      </div>
    </div>
    <!-- 平台售卖属性地方 -->
    <div
      class="type-wrap"
      v-for="(attr, index) in attrsList"
      :key="attr.attrId"
    >
      <!-- 平台售卖属性：比如说颜色 -->
      <div class="fl key">{{ attr.attrName }}</div>
      <!-- 平台相应售卖属性的属性值 -->
      <div class="fl value">
        <ul class="type-list">
          <li
            v-for="(attrValue, index) in attr.attrValueList"
            :key="index"
            @click="attrInfo(attr, attrValue)"
          >
            <a>{{ attrValue }}</a>
          </li>
        </ul>
      </div>
      <div class="fl ext"></div>
    </div>
  </div>
</template>

<script>
import { mapGetters } from "vuex";
export default {
  name: "SearchSelector",
  data() {
    return {};
  },
  computed: {
    ...mapGetters(["trademarkList", "attrsList"]),
  },
  methods: {
    //品牌的事件处理函数
    tradeMarkHandler(trademark) {
      //点击了品牌，还是需要整理参数，向服务器发请求获取数据进行展示
      //要在父组件发请求，因为父组件中的searchParams参数是带给服务器的参数，子组件需要把点击的品牌信息给父组件传过去(自定义事件)
      this.$emit("trademarkInfo", trademark);
    },
    // 平台售卖属性值的点击事件
    attrInfo(attr, attrValue) {
      this.$emit("attrInfo", attr, attrValue);
    },
  },
};
</script>

<style lang="less" scoped>
.selector {
  border: 1px solid #ddd;
  margin-bottom: 5px;
  overflow: hidden;

  .logo {
    border-top: 0;
    margin: 0;
    position: relative;
    overflow: hidden;

    .key {
      padding-bottom: 87px !important;
    }
  }

  .type-wrap {
    margin: 0;
    position: relative;
    border-top: 1px solid #ddd;
    overflow: hidden;

    .key {
      width: 100px;
      background: #f1f1f1;
      line-height: 26px;
      text-align: right;
      padding: 10px 10px 0 15px;
      float: left;
    }

    .value {
      overflow: hidden;
      padding: 10px 0 0 15px;
      color: #333;
      margin-left: 120px;
      padding-right: 90px;

      .logo-list {
        li {
          float: left;
          border: 1px solid #e4e4e4;
          margin: -1px -1px 0 0;
          width: 105px;
          height: 52px;
          text-align: center;
          line-height: 52px;
          overflow: hidden;
          text-overflow: ellipsis;
          white-space: nowrap;
          font-weight: 700;
          color: #e1251b;
          font-style: italic;
          font-size: 14px;

          img {
            max-width: 100%;
            vertical-align: middle;
          }
        }
      }

      .type-list {
        li {
          float: left;
          display: block;
          margin-right: 30px;
          line-height: 26px;

          a {
            text-decoration: none;
            color: #666;
          }
        }
      }
    }

    .ext {
      position: absolute;
      top: 10px;
      right: 10px;

      .sui-btn {
        display: inline-block;
        padding: 2px 14px;
        box-sizing: border-box;
        margin-bottom: 0;
        font-size: 12px;
        line-height: 18px;
        text-align: center;
        vertical-align: middle;
        cursor: pointer;
        padding: 0 10px;
        background: #fff;
        border: 1px solid #d5d5d5;
      }

      a {
        color: #666;
      }
    }
  }
}
</style>