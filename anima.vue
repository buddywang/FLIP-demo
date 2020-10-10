<template>
    <div>
        <div class="container">
            <div class="item" v-for="item in arr" :key="item" ref="arr">
                {{item}}
            </div>
        </div>
        <button @click="handleClick">click me</button>
    </div>
</template>
<script>
export default {
    data: ()=>{
        return {
            arr: [1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16],
            domarr: [],
            prevp: [],
            curp: []
        }
    },
    methods: {
        handleClick () {
            this.getFirst()
            console.log(1)
            this.arr = this.arr.sort(this.randomsort)
            this.$nextTick(()=>{
                this.getLast()
                this.domarr.forEach((item, index) => {
                    let prevPosition = this.prevp[index]
                    let currentPosition = this.curp[index]
                    const invert = {
                        left: prevPosition.left - currentPosition.left,
                        top: prevPosition.top - currentPosition.top,
                    }
                    const keyframes = [
                        {
                        transform: `translate(${invert.left}px, ${invert.top}px)`,
                        },
                        { transform: "translate(0)" },
                    ]

                    const options = {
                        duration: 300,
                        easing: "cubic-bezier(0,0,0.32,1)",
                    }

                    item.animate(keyframes, options)
                });
            })
        },
        randomsort() {
            return Math.random()>.5 ? -1 : 1;
            //用Math.random()函数生成0~1之间的随机数与0.5比较，返回-1或1
        },
        getFirst(){
            this.prevp = this.domarr.map((dom) => {
                const rect = dom.getBoundingClientRect()
                const { left, top } = rect
                return { left, top }
            })
        },
        getLast(){
            this.curp = this.domarr.map((dom) => {
                const rect = dom.getBoundingClientRect()
                const { left, top } = rect
                return { left, top }
            })
        }
    },
    mounted() {
        this.domarr = this.$refs.arr
    },
}
</script>
<style>
.container{
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    width: 245px;
}
.item{
    text-align: center;
    width: 50px;
    height: 50px;
    line-height: 50px;
    border: 2px solid #eee;
    margin: 5px 0;
}
</style>