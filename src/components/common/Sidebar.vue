<template>
    <div class="sidebar">
            <!-- background-color="#1e2c41"
            text-color="#bfcbd9"
            active-text-color="#20a0ff" -->
        <el-menu
            class="sidebar-el-menu"
            :default-active="onRoutes"
            :collapse="collapse"
            
            unique-opened
            router
        >
            <template v-for="item in items">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i>
                            <span slot="title">{{ item.title }}</span>
                        </template>
                        <template v-for="subItem in item.subs">
                            <el-submenu
                                v-if="subItem.subs"
                                :index="subItem.index"
                                :key="subItem.index"
                            >
                                <template slot="title">{{ subItem.title }}</template>
                                <el-menu-item
                                    v-for="(threeItem,i) in subItem.subs"
                                    :key="i"
                                    :index="threeItem.index"
                                >{{ threeItem.title }}</el-menu-item>
                            </el-submenu>
                            <el-menu-item
                                v-else
                                :index="subItem.index"
                                :key="subItem.index"
                            >{{ subItem.title }}</el-menu-item>
                        </template>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index">
                        <i :class="item.icon"></i>
                        <span slot="title">{{ item.title }}</span>
                    </el-menu-item>
                </template>
            </template>
            <template v-for="item in addminItems">
                <template v-if="item.subs">
                    <el-submenu :index="item.index" :key="item.index">
                        <template slot="title">
                            <i :class="item.icon"></i>
                            <span slot="title">{{ item.title }}</span>
                        </template>
                        <template v-for="subItem in item.subs">
                            <el-submenu
                                v-if="subItem.subs"
                                :index="subItem.index"
                                :key="subItem.index"
                            >
                                <template slot="title">{{ subItem.title }}</template>
                                <el-menu-item
                                    v-for="(threeItem,i) in subItem.subs"
                                    :key="i"
                                    :index="threeItem.index"
                                >{{ threeItem.title }}</el-menu-item>
                            </el-submenu>
                            <el-menu-item
                                v-else
                                :index="subItem.index"
                                :key="subItem.index"
                            >{{ subItem.title }}</el-menu-item>
                        </template>
                    </el-submenu>
                </template>
                <template v-else>
                    <el-menu-item :index="item.index" :key="item.index">
                        <i :class="item.icon"></i>
                        <span slot="title">{{ item.title }}</span>
                    </el-menu-item>
                </template>
            </template>
        </el-menu>
    </div>
</template>

<script>
import bus from '../common/bus';
export default {
    data() {
        return {
            collapse: false,
            items: [
                {
                    icon: 'el-icon-lx-home',
                    index: 'dashboard',
                    title: '系统首页'
                },
                {
                    icon: 'el-icon-lx-calendar',
                    index: 'table',
                    title: '新建问卷'
                },
                {
                    icon: 'el-icon-paperclip',
                    index: 'tabs',
                    title: '产品链接'
                },
                {
                    icon: 'el-icon-data-analysis',
                    index: 'donate',
                    title: '模型'
                },
                
                {
                    icon: 'el-icon-finished',
                    index: 'icon',
                    title: '预提'
                },
                {
                    icon: 'el-icon-refresh-left',
                    index: 'form',
                    title: '失败重提',
                },
                {
                    icon: 'el-icon-lx-calendar',
                    index: '3',
                    title: '表单相关',
                    subs: [
                        {
                            index: 'form',
                            title: '基本表单'
                        },
                        {
                            index: '3-2',
                            title: '三级菜单',
                            subs: [
                                {
                                    index: 'editor',
                                    title: '富文本编辑器'
                                },
                                {
                                    index: 'markdown',
                                    title: 'markdown编辑器'
                                }
                            ]
                        },
                        {
                            index: 'upload',
                            title: '文件上传'
                        }
                    ]
                },
                
            ],
            addminItems: [
                {
                    icon: 'el-icon-user',
                    index: 'charts',
                    title: '用户管理'
                },
                {
                    icon: 'el-icon-lx-global',
                    index: 'i18n',
                    title: '域名管理'
                }
                
            ]
        };
    },
    computed: {
        onRoutes() {
            return this.$route.path.replace('/', '');
        }
    },
    created() {
        // 通过 Event Bus 进行组件间通信，来折叠侧边栏
        bus.$on('collapse', msg => {
            this.collapse = msg;
            bus.$emit('collapse-content', msg);
        });
        const role = localStorage.getItem('ms_username');
        if(role == 'admin'){
            // this.items=[]
        }
        if(role != 'admin'){
            // this.addminItems=[]
        }
    }
};
</script>

<style scoped>
.sidebar {
    display: block;
    position: absolute;
    left: 0;
    top: 70px;
    bottom: 0;
    overflow-y: scroll;
}
.sidebar::-webkit-scrollbar {
    width: 0;
    /* background-color: #1e2c41; */
}
.sidebar-el-menu:not(.el-menu--collapse) {
    width: 250px;
}
.sidebar > ul {
    height: 100%;
}
</style>
