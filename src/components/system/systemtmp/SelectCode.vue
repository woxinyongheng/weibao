<template>
    <div class="selectcode">
      <p v-for="item in selectListData" v-if="selectListData.length" @click="removeList(item)">{{item.name}}</p>
    </div>
</template>

<script>
    export default {
        name: "SelectCode",
        props:['selectListData'],
        methods:{
          removeList(item){
            this.$emit('removeList',item)
          }
        }
    }
</script>

<style scoped lang="scss">
.selectcode{
  height: 200px;
  background-color: #fff;
  padding: 20px 0;
  font-size: 14px;
  p{
    margin: 10px auto;
    width: 95%;
    text-align: center;
    height: 40px;
    line-height: 40px;
    cursor: pointer;
    background-color: #999;
  }
}
</style>
