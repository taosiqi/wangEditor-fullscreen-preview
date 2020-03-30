<template>
  <!-- 富文本编辑器 -->
  <div>
    <div class="container">
      <div ref="editor" class="edit"></div>
    </div>
    <PreView :PreviewVisible="PreviewVisible" :PreviewArticle="article" @close="close"></PreView>
  </div>
</template>

<script>
import E from 'wangeditor';
import PreView from '../page/Preview';
var editor;
export default {
    name: 'editor',
    data: function() {
        return {
            PreviewVisible: false, //全屏预览
            fullscreen: false, //全屏编辑
            article:'', //编辑器内容
        };
    },
    components: {
        PreView
    },
    methods: {
        // 关闭预览
        close() {
            this.PreviewVisible = false;
        },
        // 设置编辑器文本
        GetArticle(){
            editor.txt.html("设置文本");
        }
    },
    mounted() {
        editor = new E(this.$refs.editor);
        editor.customConfig.zIndex = 100;
        editor.customConfig.pasteFilterStyle = false;
        editor.customConfig.uploadImgShowBase64 = true; // 使用 base64 保存图片
        editor.customConfig.onchange = html => {
            this.article = html;
        };
        editor.create();
        this.GetArticle();
        // 预览
        let dom = this.$refs.editor.querySelector('.w-e-toolbar');  //获取导航栏节点
        dom.appendChild(document.createElement('div'));  //增加预览节点
        let last = dom.lastChild;  //获取到预览这个节点
        last.innerHTML = '预览';  //设置预览文本
        last.classList.add('w-e-menu', 'preview');  //增加class
        let _this = this;  //添加预览点击事件
        last.addEventListener('click', function() {
            _this.PreviewVisible = !_this.PreviewVisible;
        });
        // 全屏
        dom.appendChild(document.createElement('div'));  //创建节点
        let lasttwo = dom.lastChild;  //获取全屏节点
        lasttwo.innerHTML = '全屏'; 
        lasttwo.classList.add('w-e-menu', 'preview');
        // 增加事件
        lasttwo.addEventListener('click', function() {
            let domtwo = _this.$refs.editor;  
            _this.fullscreen = !_this.fullscreen;
            if (_this.fullscreen) {
                domtwo.classList.add('fullscreen-editor');
                lasttwo.innerHTML = '退出全屏';
            } else {
                domtwo.classList.remove('fullscreen-editor');
                lasttwo.innerHTML = '全屏';
            }
        });
    }
};
</script>
<style>

.preview {
  color: #999;
}

.w-e-toolbar .w-e-menu:hover {
  z-index: 10002 !important;
}

.w-e-menu a {
  text-decoration: none;
}

.fullscreen-editor {
  position: fixed;
  width: 100vw;
  height: 100vh;
  text-align: left;
  min-height: 550px;
  z-index: 10003;
  left: 0;
  right: 0;
  top: 0;
  bottom: 0;
  background: white;
}
.fullscreen-editor > .w-e-text-container {
  height: 95% !important;
}
</style>