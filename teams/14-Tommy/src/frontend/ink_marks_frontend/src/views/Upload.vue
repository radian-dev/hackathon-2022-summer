<template>
    <div>
        <div class="crumbs">
            <el-breadcrumb separator="/">
                <el-breadcrumb-item>
                    <i class="el-icon-lx-calendar"></i> Contracts
                </el-breadcrumb-item>
                <el-breadcrumb-item>Upload contract</el-breadcrumb-item>
            </el-breadcrumb>
        </div>
        <div class="container">
            <div class="content-title">Upload contract</div>
            <div class="plugins-tips">
                <!-- Element UI自带上传组件。
                访问地址：
                <a href="http://element.eleme.io/#/zh-CN/component/upload" target="_blank">Element UI Upload</a> -->
                You can upload your ink! contract by dragging and drop to the box below.
            </div>
            <el-upload class="upload-demo"
                drag
                action="http://127.0.0.1:8888/inkcontracts"
                multiple
                :on-success="handleSuccess"
            >
                <i class="el-icon-upload"></i>
                <div class="el-upload__text">
                    drag file here or
                    <em>click to upload</em>
                </div>
                <template #tip>
                    <div class="el-upload__tip"> .contract file only and less than 500KB</div>
                </template>
            </el-upload>

            <!-- <div class="content-title">支持裁剪</div>
            <div class="plugins-tips">
                vue-cropperjs：一个封装了 cropperjs 的 Vue 组件。
                访问地址：
                <a href="https://github.com/Agontuk/vue-cropperjs" target="_blank">vue-cropperjs</a>
            </div> -->
        </div>
    </div>
</template>

<script>
import { ref } from "vue";
import VueCropper from "vue-cropperjs";
import "cropperjs/dist/cropper.css";
import defaultSrc from "../assets/img/img.jpg";
export default {
    name: "upload",
    components: {
        VueCropper,
    },
    methods: {
        handleSuccess(res, file) {
            this.$router.push({ name: 'basetable', query: { redirect: '/table' } });
        }
    },
    setup() {
        const imgSrc = ref("");
        const cropImg = ref(defaultSrc);
        const dialogVisible = ref(false);
        const cropper = ref(null);

        const setImage = (e) => {
            const file = e.target.files[0];
            // if (!file.type.includes("image/")) {
            //     return;
            // }
            const reader = new FileReader();
            reader.onload = (event) => {
                dialogVisible.value = true;
                imgSrc.value = event.target.result;
                cropper.value && cropper.value.replace(event.target.result);
            };
            reader.readAsDataURL(file);
        };

        const cropImage = () => {
            cropImg.value = cropper.value.getCroppedCanvas().toDataURL();
        };

        const cancelCrop = () => {
            dialogVisible.value = false;
            cropImg.value = defaultSrc;
        };

        return {
            cropper,
            imgSrc,
            cropImg,
            dialogVisible,
            setImage,
            cropImage,
            cancelCrop,
        };
    },
};
</script>

<style scoped>
.content-title {
    font-weight: 400;
    line-height: 50px;
    margin: 10px 0;
    font-size: 22px;
    color: #1f2f3d;
}

.pre-img {
    width: 100px;
    height: 100px;
    background: #f8f8f8;
    border: 1px solid #eee;
    border-radius: 5px;
}
.crop-demo {
    display: flex;
    align-items: flex-end;
}
.crop-demo-btn {
    position: relative;
    width: 100px;
    height: 40px;
    line-height: 40px;
    padding: 0 20px;
    margin-left: 30px;
    background-color: #409eff;
    color: #fff;
    font-size: 14px;
    border-radius: 4px;
    box-sizing: border-box;
}
.crop-input {
    position: absolute;
    width: 100px;
    height: 40px;
    left: 0;
    top: 0;
    opacity: 0;
    cursor: pointer;
}
</style>