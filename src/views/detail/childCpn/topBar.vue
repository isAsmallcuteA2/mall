<template>
    <div>
        <nav-bar>
            <div class="imgbox" slot="left" @click="back()"><img src="~assets/img/common/back.svg" alt=""></div>
        <div slot="center" class="box">
            <div v-for="(item,index) in titles" :class="{active:index==currentIndex}" @click="itemClick(index)" class="item" :key="item">{{item}}</div>
        </div>
    </nav-bar>
    </div>
</template>
<script>
import navBar from "components/common/navbar/navBar"

export default {
    data(){
        return{
            titles:["商品","参数","评论","推荐"],
            currentIndex:0
        }
    },
    components:{
        navBar,
    },
    methods:{
        itemClick(index){
            this.currentIndex=index
        },
        back(){
            this.$router.go(-1)
        }
    },
    
}
</script>
<style scoped>
.box{
        display: flex;
    font-size: 13px;
}
.item{
    flex: 1;
    /* line-height: 44px; */
}
.active{
    color: pink;
}
.imgbox img{
    margin-top: 12px;
}
</style>