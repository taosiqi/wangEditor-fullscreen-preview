<template>
    <!-- 文章预览 使用了elementUI 可以自己修改 -->
    <div>
        <el-dialog title="预览" :visible.sync="PreviewVisible" :fullscreen="true" class="PreviewVisible" :show-close="false" >
            <i class="el-icon-close" @click="close"></i>
            <div class="PreviewVisible_content" v-html="PreviewArticle">{{PreviewArticle}}</div>
        </el-dialog>
    </div>
</template>

<script>
export default {
    methods: {},
    props: {
        PreviewVisible: {
            type: Boolean,
            default: false
        },
        PreviewArticle: {
            type: String,
            default: ''
        }
    },
    methods: {
        close() {
            this.$emit('close');
        }
    },
};
</script>


<style>
.PreviewVisible {
    z-index: 101;
}
.PreviewVisible_content {
    width: 750px;
    margin: 0 auto;
}
.PreviewVisible_content >>> img,
p,
span {
    max-width: 100%;
}
.PreviewVisible .el-icon-close {
    font-size: 28px;
    position: absolute;
    right: 30px;
    top: 19px;
    cursor: pointer;
}
</style>
