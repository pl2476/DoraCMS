<template>
    <div>
        <div class="content-detail">
            <div class="readme">
                <el-row :gutter="0" class="header-main">
                    <el-col :xs="1" :sm="1" :md="1" :lg="1">
                        <div class="grid-content bg-purple">&nbsp;</div>
                    </el-col>
                    <el-col :xs="22" :sm="22" :md="22" :lg="22">
                        <el-row :gutter="24">
                            <el-col :xs="24" :sm="18" :md="18" :lg="18">
                                <div>
                                    <h2 class="content-title">{{article.doc.title}}</h2>
                                    <div class="content-author">
                                        <ul>
                                            <li class="author-name">
                                                <a>{{article.doc.author ? article.doc.author.name:''}}</a>
                                            </li>
                                            <li>
                                                <span class="dot">&nbsp;•&nbsp;</span>{{cateName}}
                                            </li>
                                            <li>
                                                <span class="dot">&nbsp;•&nbsp;</span>{{article.doc.date}}
                                            </li>
                                        </ul>
                                    </div>
                                    <div v-html="article.doc.comments">
                                    </div>
                                    <RandomArticle :articles="article.randomArticles" />
                                    <Messages :userMessageList="messages.data" :contentId="article.doc._id" />
                                </div>
                            </el-col>
                            <el-col :xs="0" :sm="6" :md="6" :lg="6" class="content-mainbody-right">
                                <div class="grid-content bg-purple-light title">
                                    <SearchBox />
                                    <CatesMenu :typeId="typeId" />
                                    <RecentContents :recentItems="recentArticle" />
                                    <HotContents :hotItems="hotlist" :typeId="$route.params.typeId" v-if="hotlist.length > 0" />
                                </div>
                            </el-col>
                        </el-row>
                    </el-col>
                    <el-col :xs="1" :sm="1" :md="1" :lg="1">
                        <div class="grid-content bg-purple">&nbsp;</div>
                    </el-col>
                </el-row>
            </div>
        </div>
    </div>
</template>

<script lang="babel">
    import {
        mapGetters
    } from 'vuex'
    import metaMixin from '~mixins'
    import Messages from '../components/Messages.vue'
    import RandomArticle from '../components/RandomArticle.vue'
    import RecentContents from '../components/RecentContents.vue'
    import SearchBox from '../components/SearchBox.vue'
    import HotContents from '../components/HotContents.vue'
    import CatesMenu from '../components/CatesMenu.vue'

    export default {
        name: 'frontend-article',
        async asyncData({ store, route }) {
            const { path, params: { id } } = route

            let params = {}, currentId = '';
            if (id) {
                if (id.indexOf('html') > 0) {
                    currentId = id.substr(0, id.length - 5);
                } else {
                    currentId = id;
                }
            }
            store.dispatch('frontend/article/getHotContentList', { typeId : 'indexPage'})
            store.dispatch('global/message/getUserMessageList',{contentId:currentId})
            store.dispatch('frontend/article/getRecentContentList')
            await store.dispatch(`frontend/article/getArticleItem`, { id: currentId, path })
        },
        mixins: [metaMixin],
        beforeRouteUpdate(to, from, next) {
            if (to.path !== from.path) this.$options.asyncData({
                store: this.$store,
                route: to
            })
            next()
        },
        computed: {
            ...mapGetters({
                article: 'frontend/article/getArticleItem',
                hotlist: 'frontend/article/getHotContentList',
                messages: 'global/message/getUserMessageList',
                recentArticle: 'frontend/article/getRecentContentList'
            }),
            cateName() {
                let catesArr = this.article.doc.categories;
                if (typeof catesArr === 'object' && catesArr.length > 1) {
                    return catesArr[catesArr.length - 1].name
                } else {
                    return '其它'
                }
            },
            typeId(){
                let catesArr = this.article.doc.categories;
                if (typeof catesArr === 'object' && catesArr.length > 1) {
                    return catesArr[0]._id
                } else {
                    return 'indexPage'
                }
            }
        },
        components: {
            Messages,
            RandomArticle,
            RecentContents,
            SearchBox,
            HotContents,
            CatesMenu
        },
        methods: {
            addTarget(content) {
                if (!content) return ''
                return content.replace(/<a(.*?)href="http/g, '<a$1target="_blank" href="http')
            }
        },
        mounted() {
            // this.$options.asyncData({store: this.$store})
        },
        metaInfo() {
            const { title, discription, tags } = this.article.doc;
            let tagArr = ['doracms'];
            if(tags){
                tagArr = tags.map((item,index)=>{
                    return item ? item.name : 'doracms'
                })
            }
            return {
                title,
                titleTemplate: '%s | 前端开发俱乐部',
                meta: [
                    { vmid: 'description', name: 'description', content: discription },
                    { vmid: 'keywords', name: 'keywords', content: tagArr.join() }
                ]
            }
        }
    }

</script>
<style lang="scss">
.content-detail {
    color: #3f3f3f;
    margin-top: 20px;
    img {
        max-width: 100% !important;
    }
    .content-title {
        margin-top: 0;
    }
    .content-author {
        color: #999999;
        ul {
            li.author-name {
                color: #20A0FF;
            }
            li {
                display: inline-block;
                margin-bottom: 10px;
                font-size: 14px;
            }
        }
    }
}
</style>
