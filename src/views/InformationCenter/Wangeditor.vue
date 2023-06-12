<template>
    <div>
      <div style="border: 1px solid #ccc; margin-top: 10px;z-index: 1000;">
        <!-- 工具栏 -->
        <Toolbar
          style="border-bottom: 1px solid #ccc"
          :editor="editor"
          :defaultConfig="toolbarConfig"
        />
        <!-- 编辑器 -->
        <Editor
          style="height: 400px; overflow-y: hidden"
          :defaultConfig="editorConfig"
          v-model="html"
          @onChange="onChange"
          @onCreated="onCreated"
        />
      </div>
    </div>
  </template>

  <script>
  import { Editor, Toolbar } from "@wangeditor/editor-for-vue";
  import '@wangeditor/editor/dist/css/style.css'
  import { Message } from 'element-ui'
  export default {
    name: "wangeditor",
    components: { Editor, Toolbar },
    props: {
      msgContent: {
        type: String,
        required:false
      },
    },
    data() {
      return {
        editor: null,
        html: this.msgContent,
        toolbarConfig: {},
        editorConfig: {
        placeholder: '请输入内容...',
        // autoFocus: false,
        // 所有的菜单配置，都要在 MENU_CONF 属性下
        MENU_CONF: {
            // 图片上传
            uploadImage: {
              server: '/dtwave-om/api/file/wangeditor/upload',
              fieldName: 'file',
              // 单个文件的最大体积限制，默认为 2M
              maxFileSize: 10 * 1024 * 1024, // 10M
              // 最多可上传几个文件，默认为 100
              maxNumberOfFiles: 10,
              // 选择文件时的类型限制，默认为 ['image/*'] 。如不想限制，则设置为 []
              allowedFileTypes: ['image/*'],
              // 自定义上传参数，例如传递验证的 token 等。参数会被添加到 formData 中，一起上传到服务端。
              meta: {
                // token: 'xxx',
                // otherKey: 'yyy'
                // file:''
              },
              // 将 meta 拼接到 url 参数中，默认 false
              metaWithUrl: false,

              // 自定义增加 http  header
              headers: {
                // Accept: 'text/x-json',
                // otherKey: 'xxx'
              },

              // 跨域是否传递 cookie ，默认为 false
              withCredentials: true,

              // 超时时间，默认为 10 秒
              timeout: 10 * 1000, //10 秒

              // 上传前
              onBeforeUpload(files) {
                Message({
                  message: '图片正在上传中,请耐心等待',
                  duration: 0,
                  customClass: 'uploadTip',
                  iconClass: 'el-icon-loading',
                  showClose: true
                });
                return files;
              },
              // 自定义插入图片
              customInsert(res, insertFn) {
                // 因为自定义插入导致onSuccess与onFailed回调函数不起作用,自己手动处理
                // 先关闭等待的Message
                Message.closeAll();
                if (res.code === 200) {
                  Message.success({
                    message: `${res.data.originalName} 上传成功`
                  });
                } else {
                  Message.error({
                    message: `${res.data.originalName} 上传失败，请重新尝试`
                  });
                }
                insertFn(res.data.link, res.data.name, res.data.link);
              },

              // 单个文件上传成功之后
              onSuccess(file, res) {
                console.log(`${file.name} 上传成功`, res);
              },

              // 单个文件上传失败
              onFailed(file, res) {
                console.log(`${file.name} 上传失败`, res);
              },

              // 上传进度的回调函数
              onProgress(progress) {
                console.log('progress', progress);
                // progress 是 0-100 的数字
              },

              // 上传错误，或者触发 timeout 超时
              onError(file, err, res) {
                console.log(`${file.name} 上传出错`, err, res);
              }
            },
            // 视频上传
            uploadVideo: {
              fieldName: 'file',
              server: '/dtwave-om/api/file/wangeditor/upload',

              // 单个文件的最大体积限制，默认为 10M
              maxFileSize: 100 * 1024 * 1024, // 100M

              // 最多可上传几个文件，默认为 5
              maxNumberOfFiles: 3,
              // 选择文件时的类型限制，默认为 ['video/*'] 。如不想限制，则设置为 []
              allowedFileTypes: ['video/*'],

              // 自定义上传参数，例如传递验证的 token 等。参数会被添加到 formData 中，一起上传到服务端。
              meta: {
                // token: 'xxx',
                // otherKey: 'yyy'
              },

              // 将 meta 拼接到 url 参数中，默认 false
              metaWithUrl: false,

              // 自定义增加 http  header
              headers: {
                // Accept: 'text/x-json',
                // otherKey: 'xxx'
              },

              // 跨域是否传递 cookie ，默认为 false
              withCredentials: true,

              // 超时时间，默认为 30 秒
              timeout: 1000 * 1000, // 1000 秒,
              // 上传之前触发
              onBeforeUpload(file) {
                Message({
                  message: '视频正在上传中,请耐心等待',
                  duration: 0,
                  customClass: 'uploadTip',
                  iconClass: 'el-icon-loading',
                  showClose: true
                });
                return file;
              },
              // 自定义插入视频
              customInsert(res, insertFn) {
                // 因为自定义插入导致onSuccess与onFailed回调函数不起作用,自己手动处理
                // 先关闭等待的Message
                Message.closeAll();
                if (res.code === 200) {
                  Message.success({
                    message: `${res.data.originalName} 上传成功`
                  });
                } else {
                  Message.error({
                    message: `${res.data.originalName} 上传失败，请重新尝试`
                  });
                }
                insertFn(res.data.link, res.data.link);
              },
              // 上传进度的回调函数
              onProgress(progress) {
                console.log(progress);
                // onProgress(progress) {       // JS 语法
                // progress 是 0-100 的数字
              },

              // // 单个文件上传成功之后
              // onSuccess(file, res) {
              //   console.log(`${file.name} 上传成功`, res);
              //   this.successMsg(file);
              // },

              // // 单个文件上传失败
              // onFailed(file, res) {
              //   console.log(`${file.name} 上传失败`, res);
              //   this.errorMsg(file);
              // },

              // 上传错误，或者触发 timeout 超时
              onError(file, err, res) {
                console.log(`${file.name} 上传出错`, err, res);
                Message.error(`${file.name} 上传失败，请重新尝试`);
              }
            },
          }
        },
      };
    },
    methods: {
      onCreated(editor) {
        this.editor = Object.seal(editor); // 【注意】一定要用 Object.seal() 否则会报错
      },
      onChange(editor) {
        this.$emit("input", editor.getHtml());
        console.log("onChange", editor.getHtml()); // onChange 时获取编辑器最新内容
      },
    },
    mounted() {},
    beforeDestroy() {
      const editor = this.editor;
      if (editor == null) return;
      editor.destroy(); // 组件销毁时，及时销毁 editor ，重要！！！
    },
  };
  </script>
  