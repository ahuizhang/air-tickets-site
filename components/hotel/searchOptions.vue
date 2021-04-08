<template>
  <el-row class="search-options">
    <el-row type="flex" class="option-row">
      <el-col :span="3">区域：</el-col>
      <el-col :class="{'hidden-all': !show}">
        <div class="scenics-box">
          <a href="#" :class="{'location-budget': 1, active: !value}" @click.prevent="_ => handleClick()">全部</a>
          <a :class="{'location-budget': true, active: value === location.id}" v-for="location in scenics" @click="_ => handleClick(location)">{{location.name}}</a>
        </div>
        <a href="#" @click="toggleShow">
          <i class="el-icon-d-arrow-right" />
          等{{scenics.length}}个区域
        </a>
      </el-col>
    </el-row>

    <el-row type="flex" class="option-row">
      <el-col :span="3">攻略:</el-col>
      <el-col>
北京，你想要的都能在这找到。博古通今，兼容并蓄，天下一城，如是帝都。

景点以故宫为中心向四处辐射；地铁便宜通畅，而且覆盖绝大多数景点。

由于早上有天安门升旗仪式，所以大多数人选择在天安门附近住宿。</el-col>
    </el-row>

    <el-row type="flex" class="option-row price-row">
      <el-col :span="3">
        均价
        <el-tooltip content="等级均价由平日价格计算得出，节假日价格会有上浮。" placement="top-start">
          <sup class="question-mark">?</sup>
        </el-tooltip>
      :</el-col>
      <el-col>
        <el-row type="flex" justify="start">
          <el-col :span="6">
            <averagePrice :starLevel="3" price="332" />
          </el-col>
          <el-col :span="6">
            <averagePrice :starLevel="4" price="521" />
          </el-col>
          <el-col :span="6">
            <averagePrice :starLevel="5" price="768" />
            </el-tooltip>
          </el-col>
        </el-row>
      </el-col>
    </el-row>
  </el-row>
</template>

<script>
  import averagePrice from './averagePrice'

  export default {
    props: [
      'defaultValue',
      'scenics', // 区域列表
      'summary', // 攻略
      'averagePriceList', // 均价列表
      'onClick',
    ],
    data() {
      const { defaultValue } = this
      const value = defaultValue ? +defaultValue : null
      return {
        value,
        show: false
      }
    },

    methods: {
      toggleShow() {
        this.show = !this.show
      },

      handleClick(location) {
        this.onClick && this.onClick(location)
        const {id = ''} = location || {}
        this.value = id
      }
    },

    components: {
      averagePrice,
    }
  }
</script>

<style scoped lang="less">
  .option-row {
    margin-bottom: 20px;
    color: #666;
    font-size: 14px;

    .location-budget {
      margin-right: 1em;
      padding: 0 2px;
      border-radius: 4px;
      display: inline-block;
      cursor: pointer;

      &:hover {
        text-decoration: underline;
        color: #09f;
      }
      &.active {
        background: #eee;
        cursor: auto;
        text-decoration: none;
        color: #999;
      }
    }

    &.price-row {
      .question-mark {
        background-color: #ccc;
        color: #fff;
        display: inline-block;
        width: 1.2em;
        height: 1.2em;
        text-align: center;
        border-radius: 100%;
      }
    }

    .el-icon-d-arrow-right {
      transform: rotate(270deg);
      color: #f90;
    }
    .hidden-all {
      .scenics-box {
        max-height: 3em;
        overflow: hidden;
      }
      .el-icon-d-arrow-right {
        transform: rotate(90deg);
      }
    }
  }
</style>
