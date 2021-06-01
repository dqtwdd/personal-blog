---
title: 手撸datePicker
tags: ["工作"]
categories: 工作
date: 2020-10-22
---

因为工作需要，自己手撸了一个 datePicker 记录一下主要的思路内容。

<!--more-->

要点：

1. 使用vant组件弹出

2. 日期和月份每天的对照关系

3. 传入一个可选日期区间，包括开始日期和结束日期，在只能选择日期区间内部的时间

```javascript
<template>
  <div>
    <van-popup
      v-model="showPicker"
      round
      closeable
      position="bottom"
      @close="close"
    >
      <div class="picker_header">
        <span @click="changeMonth(-1)">
          <svg-icon class="left_arrow" icon-class="detail" />
        </span>
        <p>{{ currYear + "年" + currMonth + "月" }}</p>
        <span @click="changeMonth(1)">
          <svg-icon class="right_arrow" icon-class="detail" />
        </span>
        <div class="border_one"></div>
      </div>
      <div class="picker_content">
        <div class="content_title">
          <p>日</p>
          <p>一</p>
          <p>二</p>
          <p>三</p>
          <p>四</p>
          <p>五</p>
          <p>六</p>
        </div>
        <div class="content_mian">
          <div
            class="content_item"
            v-for="(item, index) in currMonthDateList"
            :key="index"
            :style="item === 0 ? 'opacity:0' : ''"
            :class="isAvailable(item) ? 'date_available' : ''"
            @click="selectDate(item)"
          >
            <p>{{ item }}</p>
            <p>{{ isAvailable(item) ? "可约" : "不可约" }}</p>
          </div>
        </div>
      </div>
    </van-popup>
  </div>
</template>

<script>
export default {
  inheritAttrs: false,
  name: 'tkDatePicker',
  props: {
    showPicker: {
      type: Boolean,
      default: false
    },
    value: {
      type: String,
      default: ''
    },
    availableStart: {
      type: Array,
      default: () => {
        return []
      }
    },
    availableEnd: {
      type: Array,
      default: () => {
        return []
      }
    }
  },
  data () {
    return {
      monthStartWeek: '',
      currYear: '',
      currMonth: '',
      currDate: '',
      monthDateMap: {
        1: 31,
        2: 28,
        3: 31,
        4: 30,
        5: 31,
        6: 30,
        7: 31,
        8: 31,
        9: 30,
        10: 31,
        11: 30,
        12: 31
      },
      currMonthDateList: []
    }
  },
  mounted () {
    this.init()
  },
  methods: {
    init () {
      if (this.availableStart) {
        this.currYear = this.availableStart[0]
        this.currMonth = this.availableStart[1]
        this.currDate = this.availableStart[2]
      } else {
        const date = this.getCurrentDate()
        this.currYear = date[0]
        this.currMonth = date[1]
        this.currDate = date[2]
      }
      this.getMonthStartWeek()
    },
    // 获取当前时间
    getCurrentDate () {
      const date = new Date()
      const currYear = date.getFullYear()
      const currMonth = date.getMonth() + 1
      const currDate = date.getDate()
      return [currYear, currMonth, currDate]
    },
    // 根据当前年月计算本月一号是周几
    getMonthStartWeek () {
      this.currMonthDateList = []
      const monthStart = this.currYear + '/' + this.currMonth + '/' + '1'
      this.monthStartWeek = new Date(monthStart).getDay()
      // 填充一号前面的0
      for (let i = 1; i <= this.monthStartWeek; i++) {
        this.currMonthDateList.push(0)
      }
      // 填充
      for (let i = 1; i <= this.monthDateMap[this.currMonth]; i++) {
        if (i < 10) {
          i = '0' + i
        } else {
          i = i + ''
        }
        this.currMonthDateList.push(i)
      }
      // 闰年二月加一天
      if (
        this.currYear % 4 === 0 &&
        (this.currYear % 100 !== 0 || this.currYear % 400 === 0) &&
        this.currMonth === 2
      ) {
        this.currMonthDateList.push(29)
      }
      while (this.currMonthDateList.length < 42) {
        this.currMonthDateList.push(0)
      }
    },
    // 改月份时12月加一年，1月减一年
    changeMonth (operate) {
      if (this.currMonth === 1 && operate === -1) {
        this.currYear = this.currYear - 1
        this.currMonth = 12
      } else if (this.currMonth === 12 && operate === 1) {
        this.currYear = this.currYear + 1
        this.currMonth = 1
      } else {
        this.currMonth = this.currMonth + operate
      }
      this.getMonthStartWeek()
    },
    // 计算日期是否是可选的
    isAvailable (date) {
      const currDate = new Date(this.currYear, this.currMonth, date)
      const dateStart = new Date(
        this.availableStart[0],
        this.availableStart[1],
        this.availableStart[2]
      )
      const dateEnd = new Date(
        this.availableEnd[0],
        this.availableEnd[1],
        this.availableEnd[2]
      )
      return currDate >= dateStart && currDate <= dateEnd
    },
    // 选择日期
    selectDate (date) {
      if (!this.isAvailable(date)) {
        return
      }
      const dateArr = [this.currYear, this.currMonth, date * 1]
      this.$emit('selectDate', dateArr)
      this.close()
    },
    close () {
      this.$emit('close')
    }
  }
}
</script>
```
