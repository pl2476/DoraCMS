<template>
    <div>
        <el-table align="center" v-loading="loading" ref="multipleTable" :data="dataList" tooltip-effect="dark" style="width: 100%" @selection-change="handleSystemLogsSelectionChange">
            <el-table-column type="selection" width="55">
            </el-table-column>
            <el-table-column prop="logs" label="行为">
            </el-table-column>
            <el-table-column prop="type" label="类别">
                <template scope="scope">
                    <span v-if="scope.row.type == 'login'">系统登录</span>
                </template>
            </el-table-column>
            <el-table-column prop="date" label="发生时间">
            </el-table-column>
            <el-table-column label="操作" width="150">
                <template scope="scope">
                    <el-button size="mini" type="danger" icon="delete" @click="deleteDataItem(scope.$index, dataList)">删除</el-button>
                </template>
            </el-table-column>
        </el-table>
    </div>
</template>

<script>
import services from '../../store/services.js';
export default {
    props: {
        dataList: Array
    },
    data() {
        return {
            loading: false,
            multipleSelection: []
        }
    },

    methods: {
        handleSystemLogsSelectionChange(val) {
            if (val && val.length > 0) {
                let ids = val.map((item, index) => {
                    return item._id;
                })
                this.multipleSelection = ids;
                this.$emit('changeSystemLogsSelectList', ids);
            }
        },
        deleteDataItem(index, rows) {
            this.$confirm('此操作将永久删除该记录, 是否继续?', '提示', {
                confirmButtonText: '确定',
                cancelButtonText: '取消',
                type: 'warning'
            }).then(() => {
                return services.deleteSystemOptionLogs({
                    ids: rows[index]._id,
                });
            }).then((result) => {
                if (result.data.state === 'success') {
                    this.$store.dispatch('getSystemLogsList');
                    this.$message({
                        message: '删除成功',
                        type: 'success'
                    });
                } else {
                    this.$message.error(result.data.message);
                }
            }).catch(() => {
                this.$message({
                    type: 'info',
                    message: '已取消删除'
                });
            });
        }
    }
}

</script>
