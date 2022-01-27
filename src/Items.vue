<template>
    <section id="image-container " class="mb-8 mx-auto flex flex-wrap justify-content-start   -ml-3 h-auto">
        <div v-for="(item, index) in items"  style=" width: 100px; height: 100px;" class="overflow-hidden  relative image-box mt-3 ml-3 block border-2 shadow-xl rounded-md">

            <!--      preview images     -->
            <template v-if="item.type == 'image'" >
            <div  class="absolute h-full w-full bg-gray-100 rounded opacity-30" :class="(item.status == 'uploaded') && 'hidden'"></div>
            <div class="absolute tool-bar-img flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg " :class="(item.status == 'uploaded') && 'hidden'">
                <i v-if="item.status == 'uploading'" @click="cancelUpload(index)"  title="Cancel Upload" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-ban "></i>
                <i v-if="item.status == 'pending' || item.status == 'cancel'"  @click="destroy(index, item.url, item.type)"  title="Delete" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-trash-alt" ></i>

            </div>

            <img style="height: 100px;" width="100px" height="100px" class="upload-images object-cover " :src="item.url" >
            <circle-progress v-if="item.status == 'uploading'" :percent="item.percent" class="absolute "></circle-progress>

            </template >

            <!--      preview Videos     -->

            <template v-else-if="item.type == 'video'">
                <div  class="absolute h-full w-full bg-gray-100 rounded opacity-30" :class="(item.status == 'uploaded') && 'hidden'"></div>
                <div class="z-10 absolute tool-bar-img flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg " :class="(item.status == 'uploaded') && 'hidden'">
                    <i v-if="item.status == 'uploading'" @click="cancelUpload(index)"  title="Cancel Upload" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-ban "></i>
                    <i v-if="item.status == 'pending' || item.status == 'cancel'"  @click="destroy(index, item.url, item.type)"  title="Delete" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-trash-alt" ></i>

                </div>

                <video class=" object-cover" style="height: 100px;" width="100px" height="100px" autoplay loop muted playsinline>
                    <source :src="item.url" />
                </video>
                <circle-progress v-if="item.status == 'uploading'" :percent="item.percent" class="absolute "></circle-progress>
            </template>

            <!--      preview Files     -->
            <template v-else>
                <div  class="absolute h-full w-full bg-gray-100 rounded opacity-30" :class="(item.status == 'uploaded') && 'hidden'"></div>
                <div class="absolute tool-bar-img flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg " :class="(item.status == 'uploaded') && 'hidden'">
                    <i v-if="item.status == 'uploading'" @click="cancelUpload(index)"  title="Cancel Upload" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-ban "></i>
                    <i v-if="item.status == 'pending' || item.status == 'cancel'"  @click="destroy(index, item.url, item.type)"  title="Delete" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-trash-alt" ></i>

                </div>

                <img style="height: 100px;" width="100px" height="100px" class="upload-images object-cover " :src="fileSrc" >
                <circle-progress v-if="item.status == 'uploading'" :percent="item.percent" class="absolute "></circle-progress>
            </template>

        </div>
    </section>
</template>

<script>
import CircleProgress from "./CircleProgress";
import fileSrc from "./../art/file.png"


export default {
    props: ['items', 'files'],
    data:function(){
      return {
          fileSrc: fileSrc
      }
    },
    components:{
        'circle-progress': CircleProgress,
    },
        methods: {
            destroy: function(index, file, type){

                URL.revokeObjectURL(file);
                this.items.splice(index, 1);
                this.files.splice(index, 1);
                if(!this.$parent.isAllUploading()) this.$parent.$data.uploading = false;

            },

            cancelUpload: function(index){

                this.items[index].source.cancel();
                this.items[index].status = 'cancel';
                console.log(this.$parent.isAllUploading())
                if(!this.$parent.isAllUploading()) this.$parent.$data.uploading = false;
            },
            isAllNullExceptOne: function(){

                let result =  this.files.reduce((total, cur) => Number(Boolean(cur)) + total)
                return result == 1;
            },

        }
}
</script>
