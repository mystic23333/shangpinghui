<template>
  <div class="pagination">
    <!-- 上部 -->
    <button :disabled="pageNo === 1" @click="$emit('getPageNo', pageNo - 1)">
      上一页
    </button>
    <button
      v-show="startNumAndEndNum.start > 1"
      @click="$emit('getPageNo', 1)"
      :class="{ active: pageNo == 1 }"
    >
      1
    </button>
    <button v-show="startNumAndEndNum.start > 2">...</button>

    <!-- 中间的连续页码部分 -->
    <button
      v-for="(page, index) in continues"
      :key="index"
      @click="$emit('getPageNo', startNumAndEndNum.start + page - 1)"
      :class="{ active: pageNo == startNumAndEndNum.start + page - 1 }"
    >
      {{ startNumAndEndNum.start + page - 1 }}
    </button>

    <!-- 下部 -->
    <button v-show="startNumAndEndNum.end < totalPage - 1">...</button>
    <button
      v-show="startNumAndEndNum.end < totalPage"
      @click="$emit('getPageNo', totalPage)"
      :class="{ active: pageNo == totalPage }"
    >
      {{ totalPage }}
    </button>
    <button
      :disabled="pageNo === totalPage"
      @click="$emit('getPageNo', pageNo + 1)"
    >
      下一页
    </button>

    <button style="margin-left: 30px">共{{ total }}条</button>
  </div>
</template>

<script>
export default {
  name: "Pagination",
  data() {
    return {};
  },
  props: ["pageNo", "pageSize", "total", "continues"],
  computed: {
    //总共有多少页
    totalPage() {
      return Math.ceil(this.total / this.pageSize); //页数要向上取整
    },
    //计算出中间连续页码的起始数字和结束数字（连续页码至少有5页）
    startNumAndEndNum() {
      const { pageNo, continues, totalPage } = this;
      //先定义两个变量存储起始数字与结束数字
      let start = 0,
        end = 0;
      //连续页码至少为5页，如果出现不正常现象（即不够5页）
      if (totalPage < continues) {
        start = 1;
        end = this.totalPage;
      } else {
        //正常现象，总页数大于等于5
        start = pageNo - parseInt(continues / 2); //parseInt为取整函数
        end = pageNo + parseInt(continues / 2);
        //把出现的不正常现象纠正（start为0或负数）
        if (start < 1) {
          start = 1;
          end = continues;
        }
        //end数字大于总页数
        if (end > totalPage) {
          end = totalPage;
          start = totalPage - continues + 1;
        }
      }
      return { start, end };
    },
  },
  methods: {},
};
</script>

<style scoped>
.pagination {
  text-align: center;
}
.pagination button {
  margin: 0 5px;
  background: #f4f4f5;
  color: #606266;
  outline: none;
  border-radius: 2px;
  padding: 0 4px;
  vertical-align: top;
  font-size: 13px;
  min-width: 35.5px;
  height: 28px;
  line-height: 28px;
  cursor: pointer;
  box-sizing: border-box;
  text-align: center;
  border: 0;
}
.pagination button:disabled {
  color: #c0c4cc;
  cursor: not-allowed;
}
.pagination .active {
  cursor: not-allowed;
  background: #409eff;
  color: #fff;
}
</style>
