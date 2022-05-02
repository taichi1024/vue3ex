<script setup lang="ts">
import { reactive, computed } from 'vue';

type ShoppingCart = {
  name: string,
  num: number,
  price: number,
}
const shoppingCarts = reactive<ShoppingCart[]>([
    {
      name: '电脑',
      num: 1,
      price: 10000,
    },
    {
      name: '鼠标',
      num: 2,
      price: 10,
    },
    {
      name: '键盘',
      num: 3,
      price: 40,
    },
  ]
)

const computePrice = (item: ShoppingCart, flag: number) => {
  if (flag === 1) {
    item.num++
  }
  if (flag === 2 && item.num > 1) {
    item.num--
  }
}

const del = (index: number) => {
  shoppingCarts.splice(index, 1)
}

const total = computed<number>(() => {
  return shoppingCarts.reduce((prev, next) => {
    return prev + (next.num * next.price)
  }, 0)
})
</script>

<template>
  <table>
    <thead>
      <tr>
        <th>商品名称</th>
        <th>购买数量</th>
        <th>总价</th>
        <th>操作</th>
      </tr>
    </thead>
    <tbody>
      <tr :key="index" v-for="(item,index) in shoppingCarts">
        <td>{{ item.name }}</td> 
        <td>
          {{ item.num }}
          <button @click="computePrice(item, 1)">+</button>
          <button @click="computePrice(item, 2)">-</button>
        </td> 
        <td>{{ item.num * item.price }}</td> 
        <td><button @click="del(index)">删除</button></td>
      </tr>
    </tbody>
    <tfoot>
      <tr>
        <td colspan="3">合计：{{ total }}</td>
      </tr>
    </tfoot>
  </table>
</template>

<style scoped>
table, tr, th, td {
  border: 1px solid #ccc;
  border-collapse: collapse;
}
</style>
