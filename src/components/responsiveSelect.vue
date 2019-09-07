<template>
    <div>
        <p>商圈门店</p>
        <div class="container">
            <div >
                <div>
                    <input type="search" placeholder="请输入门店名称"><button>搜索</button>
                </div>

                <div><span>全选</span><input type="checkbox" v-model="checkAll"/></div>
                <ul>
                    <li v-for="(item) in data" :key="item.keyName">
                        <div><span>{{item.name}}</span><input type="checkbox" v-model="item.check"/></div>
                        <ul>
                            <li v-for="(itemA) in item.list" :key="itemA.keyName">
                                <span>{{itemA.name}}</span><input type="checkbox" v-model="itemA.check"/>
                            </li>
                        </ul>
                    </li>
                </ul>
            </div>
            <div>
                <ul>
                    <li v-for="(item) in data" :key="item.keyName" :style="item.list | showTitle">
                        <div><span>{{item.name}}</span><input type="checkbox" :checked="true" @click="deleteList(item)"></div>
                        <ul>
                            <li v-for="(itemA) in item.list" v-show="itemA.check" :key="itemA.keyName">
                                <span>{{itemA.name}}</span><input type="checkbox" v-model="itemA.check">
                            </li>
                        </ul>
                    </li>
                </ul>
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
        check: false,
        list: [
            {
                name: '第一分组',
                keyName: 'firstGroups',
                check: false,
            },
            {
                name: '第二分组',
                keyName: 'secondGroups',
                check: false,
            }
        ]
    },
    {   
        name: '购物',
        keyName: 'shopping',
        check: false,
        list: [
            {
                name: '第一分组',
                keyName: 'firstGroups',
                check: false,
            },
            {
                name: '第二分组',
                keyName: 'secondGroups',
                check: false,
            },
            {
                name: '第二分组',
                keyName: 'thirdGroups',
                check: false,
            }
        ]
    },
];

Array.isArray(data);

export default {
    name: "index",
    data(){
        return {
            data: data,         // 从模拟数据引入的 data
            oldData: [],        // 旧数据
            checkedData: [],    // 记录右边展示的数据(但其实并不是用这个数据做展示的)
            checkAll: false,    // 记录全选
        }
    },
    computed: {
        
    },
    watch:{
        // 监听 data 变化
        data: {
            handler(data){
                data.forEach((item, i) => {
                    if(item.check !== this.oldData[i].check){
                        item.list.forEach(listItem => {
                            listItem.check = item.check;
                        })
                    }
                    else{
                        let checkAll = true;
                        item.list.forEach(listItem => {
                            if(!listItem.check){
                                checkAll = false;
                            }
                        })
                        item.check = checkAll;
                    }
                });
                let checkAll = true;
                data.forEach(item => {
                    if(!item.check){
                        checkAll = false;
                    }
                })
                this.checkAll = checkAll;
                this.oldData = JSON.parse(JSON.stringify(data))
                // 过滤右边显示的数据
                this.checkDataWatcher(data)
            },
            deep: true,
        },
        // 监听全选变化
        checkAll(val){
            this.data.forEach(item => item.check = val)
        }
    },
    created() {
        // 给拿到的数据添加 check 属性
        this.data.forEach(item => {
            this.$set(item, 'check', false)
            item.list.forEach(item => {
                this.$set(item, 'check', false)
            });
        });
        // 把原始数据存起来，用来对比变化
        this.oldData = JSON.parse(JSON.stringify(this.data));
    },
    methods: {
        checkDataWatcher(data){
            let newData = [];
            data = JSON.parse(JSON.stringify(data)); // 先复制下来，不能改动原来的数据
            data.forEach((item, i) => {
                if(item.check){
                    newData.push(item);
                    return;
                }
                let checkNone = true;
                let newItem = item;
                newItem.list = item.list.filter(listItem => {
                    if(listItem.check){
                        checkNone = false;
                    }
                    return listItem.check;
                });
                !checkNone && newData.push(newItem);
            });
            console.log(newData);
            this.checkedData = newData;
        },
        deleteList(item){
            console.log(item);
            item.list.forEach(item => {
                item.check = false;
            })
        }
    },
    filters: {
        showTitle(item){
            let itemNoneSelect = 'display: none;';
            Array.isArray(item) && item.forEach(item => {
                console.log(item.check)
                if(item.check){
                    itemNoneSelect = "display: block;"
                }
            });
            return itemNoneSelect
        }
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