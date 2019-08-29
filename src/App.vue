<template>
  <div id="app">
    <h1>电影抢座</h1>
    <div>已选座位：</div>
    <ul>
      <li v-if="text.length === 0 ? true : false  ">暂未选择座位</li>
      <li v-for="(item, index) in text" :key="index">{{item.value}}</li>
    </ul>
    <div>剩余座位：</div>
    <ul class="sy">
      <li v-for="item in list" :key="item.id" @click="show(item.id, $event)">{{item.zw}}</li>
      <!-- <li>2排2座</li>
      <li>2排3座</li>
      <li>2排4座</li> -->
    </ul>
    <form class="form" v-if="text.length === 0 ? true : false  ">
      取票人：<input type="text"> <br>
      手机号：<input type="text">
      <strong></strong>
    </form>
    <form class="form" v-for="(item, index) in text" :key="index">
      取票人：<input type="text" v-model="item.name"> <br>
      手机号：<input type="text" v-model="item.password">
      <strong>{{item.value}}</strong>
    </form>
    <button @click="ok">提交订单</button>
  </div>
</template>

<script>
export default {
  data () {
    return {
      list: [
        { zw: '2排1座', id: 1 },
        { zw: '2排2座', id: 2 },
        { zw: '2排3座', id: 3 },
        { zw: '2排4座', id: 4 }
      ],
      current: 0,
      text: [],
      flag: true
    }
  },
  methods: {
    show (id, e) {
      if (e.target.className === '') {
        this.flag = true
      } else {
        this.flag = false
      }
      if (this.flag) {
        if (e.target.className.indexOf('') === -1) {
          e.target.className = '' // 切换按钮样式
        // 写逻辑
        } else {
          e.target.className = 'cur'// 切换按钮样式
        // 写逻辑
        }

        console.log(e.toElement.innerText)
        let str = e.toElement.innerText
        if (this.text.length === this.list.length) {
          return
        }
        this.text.push({ value: str })
        var hash = []
        for (var i = 0; i < this.text.length; i++) {
          for (var j = i + 1; j < this.text.length; j++) {
            if (this.text[i].value === this.text[j].value) {
              ++i
            }
          }
          hash.push(this.text[i])
        }
        this.text = hash
      } else {
        if (e.target.className.indexOf('') === -1) {
          e.target.className = 'cur' // 切换按钮样式
        // 写逻辑
        } else {
          e.target.className = ''// 切换按钮样式
        // 写逻辑
        }
        var index = this.text.findIndex(value => value.value === e.toElement.innerText)
        console.log(index)
        this.text.splice(index, 1)
      }
    },
    ok () {
      if (this.text.length === 0) {
        return alert('请选择座次')
      }
      console.log(this.text)

      let username = []
      let password = []
      this.text.forEach((item, index) => {
        if (item.name === undefined || item.password === undefined) {
          username.push()
          password.push()
        } else {
          username.push(item.name)
          password.push(item.password)
        }
      })
      console.log(username)

      if (username.length !== this.text.length) {
        return alert('请输入取票人')
      }
      if (password.length !== this.text.length) {
        return alert('请输入手机号')
      }
      let nameobj = username.reduce(function (prev, next) {
        prev[next] = (prev[next] + 1) || 1
        return prev
      }, {})
      let passobj = password.reduce(function (prev, next) {
        prev[next] = (prev[next] + 1) || 1
        return prev
      }, {})
      this.text.forEach((item, index) => {
        if (nameobj[item.name] === 2 || passobj[item.password] === 2) {
          throw alert('取票人与手机号重复')
        }
      })
      return alert('提交成功')
    }
  }
}
</script>

<style>
body{
  height: 947px;
  padding: 0;
  margin: 0;
}
#app {
  width: 400px;
  margin: auto;
}
ul li {
  display: inline-block;
  height: 40px;
  border: 1px solid #ccc;
  margin: 10px;
  padding: 10px;
  line-height: 40px;
  border-radius: 40%;
}

.sy li:hover {
  background: #999;
}
.cur {
  background: #999;
}
button {
  width: 400px;
  height: 50px;
  background-color: red;
  border: 0;
  outline:none;
  position: absolute;
  bottom: 0;
}
.form {
  border: 1px solid #ccc;
  border-radius: 20%;
  padding: 10px;
}
</style>
