<template xmlns="http://www.w3.org/1999/html">
    <div>
        <div class="container">
            <div class="handle-box">
                <el-select v-model="query.address" placeholder="地址" class="handle-select mr10">
                    <el-option key="1" label="广东省" value="广东省"></el-option>
                    <el-option key="2" label="湖南省" value="湖南省"></el-option>
                </el-select>
                <el-input v-model="query.name" placeholder="用户名" class="handle-input mr10"></el-input>
                <el-button type="primary" :icon="Search" @click="handleSearch">搜索</el-button>
                <el-button type="primary" :icon="Plus">新增</el-button>
            </div>
            <el-table :data="tableData" border class="table" ref="multipleTable" header-cell-class-name="table-header">

                <el-table-column prop="id" label="性别" width="55" align="center"></el-table-column>
                <el-table-column label="头像" align="center">
                    <template #default="scope">
                        <el-image
                                class="table-td-thumb"
                                :src="scope.row.thumb"
                                :z-index="10"
                                :preview-src-list="[scope.row.thumb]"
                                preview-teleported
                        >
                        </el-image>
                    </template>
                </el-table-column>
                <el-table-column prop="name" label="用户名"></el-table-column>
                <el-table-column label="电话">
                    <template #default="scope">￥{{ scope.row.money }}</template>
                </el-table-column>

                <el-table-column prop="address" label="客户地址"></el-table-column>
                <el-table-column label="录入人" align="center">
                    <template #default="scope">
                        <el-tag
                                :type="scope.row.state === '成功' ? 'success' : scope.row.state === '失败' ? 'danger' : ''"
                        >
                            {{ scope.row.state }}
                        </el-tag>
                    </template>
                </el-table-column>

                <el-table-column prop="date" label="客户录入时间"></el-table-column>
                <el-table-column label="操作" width="290" align="center">
                    <template #default="scope">
                        <el-button text :icon="Edit" @click="handleEdit(scope.$index, scope.row)" v-permiss="15">
                            编辑
                        </el-button>
                        <el-button text :icon="Delete" class="red" @click="handleDelete(scope.$index)" v-permiss="16">
                            删除
                        </el-button>
                        <el-button  text :icon="Search" type="primary"  @click="drawer = true" v-permiss="17">
                            详情
                        </el-button>
                    </template>
                </el-table-column>
            </el-table>
            <div class="pagination">
                <el-pagination
                        background
                        layout="total, prev, pager, next"
                        :current-page="query.pageIndex"
                        :page-size="query.pageSize"
                        :total="pageTotal"
                        @current-change="handlePageChange"
                ></el-pagination>
            </div>
        </div>

        <!-- 编辑弹出框 -->
        <el-dialog title="编辑" v-model="editVisible" width="30%" >
            <el-form label-width="70px">
                <el-form-item label="用户名">
                    <el-input v-model="form.name"></el-input>
                </el-form-item>
                <el-form-item label="地址">
                    <el-input v-model="form.address"></el-input>
                </el-form-item>
                <el-form-item label="电话">
                    <el-input v-model="form.money"></el-input>
                </el-form-item>
                <el-form-item label="录入时间">
                    <el-input v-model="form.date"></el-input>
                </el-form-item>
                <el-form-item label="录入人">
                    <el-input v-model="form.state"></el-input>
                </el-form-item>
            </el-form>
            <template #footer>
				<span class="dialog-footer">
					<el-button @click="editVisible = false">取 消</el-button>
					<el-button type="primary" @click="saveEdit">确 定</el-button>
				</span>
            </template>
        </el-dialog>

<!--        //右侧导航栏-->
        <el-drawer
                v-model="drawer"
                title="客户详情"
                :direction="direction"
                :before-close="handleClose"
                :size="700"
        >
            <el-button class="btn-dig" >
                客户资料
            </el-button>
            <el-button class="btn-dig">
                跟进联系
            </el-button>
            <el-button class="btn-dig">
                对话模块
            </el-button>

            <br/><br/><br/>
            <div v-if="ShowCustomerFlag" class="Show1" >
                <el-form label-width="70px" >
<!--                    <div style="border-bottom: 2px solid #ccc;"></div><br/>-->
                    <el-form-item label="用户名" >
                        {{"张三"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="地址">
                        {{"广东省东莞市长安镇"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="电话">
                        {{"123"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="录入时间">
                        {{"2019-11-1"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="录入人">
                        {{"2019-11-1"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="客户备注">
                        {{"这个人工作很认真负责任，值得好好深入发展！喜欢喝咖啡。不吃香菜爱香水,"+
                            "性格热情开朗，待人友好，为人诚实谦虚。工作勤奋，认真负责，能吃苦耐劳,"+
                    "尽职尽责，有耐心。具有亲和力，平易近人，善于与人沟通。"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                </el-form>
            </div>

            <div v-if="ContactFlag" class="Show1" >
                <el-form label-width="70px" >
                    <!--                    <div style="border-bottom: 2px solid #ccc;"></div><br/>-->
                    <el-form-item label="用户名" >
                        {{"张三"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="地址">
                        {{"广东省东莞市长安镇"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="电话">
                        {{"123"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="录入时间">
                        {{"2019-11-1"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="录入人">
                        {{"2019-11-1"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                    <el-form-item label="客户备注">
                        {{"这个人工作很认真负责任，值得好好深入发展！喜欢喝咖啡。不吃香菜爱香水,"+
                    "性格热情开朗，待人友好，为人诚实谦虚。工作勤奋，认真负责，能吃苦耐劳,"+
                    "尽职尽责，有耐心。具有亲和力，平易近人，善于与人沟通。"}}
                    </el-form-item>
                    <div style="border-bottom: 2px solid #ccc;"></div><br/>
                </el-form>
            </div>

<!--            <span>Hi, there!</span>-->
        </el-drawer>

        <el-drawer v-model="drawer2" :direction="direction">
<!--            <template #header>-->
<!--                <h4>set title by slot</h4>-->
<!--            </template>-->
            <template #footer>
                <div style="flex: auto;">
                    <el-button @click="cancelClick">cancel</el-button>
                    <el-button type="primary" @click="confirmClick">confirm</el-button>
                </div>
            </template>
        </el-drawer>


    </div>
</template>





<script setup lang="ts" name="basetable">
    import { ref, reactive } from 'vue';
    import { ElMessage, ElMessageBox } from 'element-plus';
    import { Delete, Edit, Search, Plus } from '@element-plus/icons-vue';
    import { fetchData } from '../api/index';

    // import { ref } from 'vue'
    // import { ElMessageBox } from 'element-plus'

    const drawer = ref(false)
    const drawer2 = ref(false)
    const direction = ref('rtl')
    const radio1 = ref('Option 1')
    const ShowCustomerFlag = ref(true)
    const ContactFlag = ref(false)



    function cancelClick() {
        drawer2.value = false
    }
    function confirmClick() {
        ElMessageBox.confirm(`Are you confirm to chose ${radio1.value} ?`)
            .then(() => {
                drawer2.value = false
            })
            .catch(() => {
                // catch error
            })
    }



    const handleClose = (done: () => void) => {
        ElMessageBox.confirm('是否关闭客户详情页面？')
            .then(() => {
                done()
            })
            .catch(() => {
                // catch error
            })
    }

    interface TableItem {
        id: number;
        name: string;
        money: string;
        state: string;
        date: string;
        address: string;
    }

    const query = reactive({
        address: '',
        name: '',
        pageIndex: 1,
        pageSize: 10
    });
    const tableData = ref<TableItem[]>([]);
    const pageTotal = ref(0);
    // 获取表格数据
    const getData = () => {
        fetchData().then(res => {
            tableData.value = res.data.list;
            pageTotal.value = res.data.pageTotal || 50;
        });
    };
    getData();

    // 查询操作
    const handleSearch = () => {
        query.pageIndex = 1;
        getData();
    };
    // 分页导航
    const handlePageChange = (val: number) => {
        query.pageIndex = val;
        getData();
    };

    // 删除操作
    const handleDelete = (index: number) => {
        // 二次确认删除
        ElMessageBox.confirm('确定要删除吗？', '提示', {
            type: 'warning'
        })
            .then(() => {
                ElMessage.success('删除成功');
                tableData.value.splice(index, 1);
            })
            .catch(() => {});
    };

    // 表格编辑时弹窗和保存
    const editVisible = ref(false);
    let form = reactive({
        name: '',
        address: '',
        money:'',
        state:'',
        date:'',
    });
    let idx: number = -1;
    const handleEdit = (index: number, row: any) => {
        idx = index;
        form.name = row.name;
        form.address = row.address;
        form.date=row.date;
        form.money = row.money;
        form.state = row.state;
        editVisible.value = true;
    };
    const saveEdit = () => {
        editVisible.value = false;
        ElMessage.success(`修改第 ${idx + 1} 行成功`);
        tableData.value[idx].name = form.name;
        tableData.value[idx].address = form.address;
    };
</script>

<style scoped>
    .handle-box {
        margin-bottom: 20px;
    }

    .handle-select {
        width: 120px;
    }

    .handle-input {
        width: 300px;
    }
    .table {
        width: 100%;
        font-size: 14px;
    }
    .red {
        color: #F56C6C;
    }
    .mr10 {
        margin-right: 10px;
    }
    .table-td-thumb {
        display: block;
        margin: auto;
        width: 40px;
        height: 40px;
    }
    .btn-dig {
        width: 8em;
        position: relative;
        height: 2.5em;
        border: 3px ridge #B4D9FF;

    }

    /*.btn-dig::after {*/
    /*    content: "";*/
    /*    position: absolute;*/
    /*    top: -10px;*/
    /*    left: 3%;*/
    /*    width: 95%;*/
    /*    height: 40%;*/
    /*    background-color: #FFFFFF;*/
    /*    transition: 0.5s;*/
    /*    transform-origin: center;*/
    /*}*/

    /*.btn-dig::before {*/
    /*    content: "";*/
    /*    transform-origin: center;*/
    /*    position: absolute;*/
    /*    top: 80%;*/
    /*    left: 3%;*/
    /*    width: 95%;*/
    /*    height: 40%;*/
    /*    background-color: #FFFFFF;*/
    /*    transition: 0.5s;*/
    /*}*/

    /*.btn-dig:hover::before, button:hover::after {*/
    /*    transform: scale(0)*/
    /*}*/

    /*.btn-dig:hover {*/
    /*    box-shadow: inset 0px 0px 10px #B4D9FF;*/
    /*}*/
    #el-drawer_header {
        FONT-WEIGHT: 500;
        align-items: center;
        color: #246bc3;
        display: flex;
        margin-bottom: 32px;
        padding: var(--el-drawer-padding-primary);
        padding-bottom: 0;
    }
    .Show1{
        width: 100%;
        height: 85%;
        padding: 30px 50px ;
        /*transition: .2s linear;*/
        border: 2.5px solid black;
        font-size: 14px;
        text-transform: uppercase;
        /*letter-spacing: 1px;*/
    }
    /*.picture{*/
    /*    width: 50px;*/
    /*    !*border: 10px #333333;*!*/
    /*    border-radius:50%;*/
    /*    float: left;*/
    /*    position: absolute;*/
    /*    !*border-width: 100px;*!*/
    /*    !*padding-left: 20px;*!*/
    /*}*/
</style>
