<template>
    <div>
        <p>商圈门店</p>
        <div class="container">
            <div >
                <div>
                    <input type="search" placeholder="请输入门店名称"><button>搜索</button>
                </div>

                <div><span>全选</span><input type="checkbox" v-model="checkAll"></div>
                <ul>
                    <li v-for="(item) in data" :key="item.keyName">
                        <div><span>{{item.name}}</span><input type="checkbox" v-model="item.check"></div>
                        
                        <ul>
                            <li v-for="(itemA) in item.list" :key="itemA.keyName">
                                <span>{{itemA.name}}</span><input type="checkbox" v-model="itemA.check">
                            </li>
                        </ul>
                    </li>
                </ul>
            </div>
            <div>
                
            </div>
        </div>
       
    </div>
</template>

<script>
/* eslint-disable */
// 模拟数据
let data = [
    {   
        name: '餐饮',
        keyName: 'food',
        list: [
            {
                name: '第一分组',
                keyName: 'firstGorups'
            },
            {
                name: '第二分组',
                keyName: 'secondGorups'
            }
        ]
    },
    {   
        name: '购物',
        keyName: 'shopping',
        list: [
            {
                name: '第一分组',
                keyName: 'firstGorups'
            },
            {
                name: '第二分组',
                keyName: 'secondGorups'
            }
        ]
    },
]

Array.isArray(data)

export default {
    name: "index",
    data(){
        return {
            data: data,         // 从模拟数据引入的 data
            checkList: [],      // 记录选中的列表
            checkAll: false,    // 记录全选
        }
    },
    computed: {
        checkedData(){
            let data = JSON.parse(JSON.stringify(this.data));
            data = data.filter(item => {
                if(item.check){
                    return true;
                }
                let checkNone = true;
                item = item.list.filter(item => {
                    if(item.check){
                        checkNone = false;
                    }
                    return item.check
                })
                return !checkNone;
            })
            console.log(data);
            return data;
        }
    },
    created() {
        this.data.forEach(item => {
            this.$set(item, 'check', false)
            // item.check = false;
            item.list.forEach(item => {
                this.$set(item, 'check', false)
                // item.check = false;
            });
        })
        console.log(this.data)
    } 
}
</script>


<style scoped>
    .container {
        display: flex;
    }
    .container > div:nth-child(1) {
        border-right: 1px solid #ddd;
        padding: 20px;

    }
</style>