<template>
    <section id="image-container " class="mb-8 mx-auto flex flex-wrap justify-content-start   -ml-3 h-auto">
        <div v-for="(item, index) in items"  style=" width: 100px; height: 100px;" class="overflow-hidden  relative image-box mt-3 ml-3 block border-2 shadow-xl rounded-md">

            <!--      preview images     -->
            <template v-if="item.type == 'image'" >
                <div  class="absolute h-full w-full bg-gray-100 rounded opacity-30" :class="(item.status == 'uploaded') && 'hidden'"></div>
                <div class="absolute tool-bar-img flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg " :class="(item.status == 'uploaded') && 'hidden'">
                    <svg v-if="item.status == 'uploading'" @click="cancelUpload(index)" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400  "  title="Cancel Upload" height="16px" width="16px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M256 8C119.034 8 8 119.033 8 256s111.034 248 248 248 248-111.034 248-248S392.967 8 256 8zm130.108 117.892c65.448 65.448 70 165.481 20.677 235.637L150.47 105.216c70.204-49.356 170.226-44.735 235.638 20.676zM125.892 386.108c-65.448-65.448-70-165.481-20.677-235.637L361.53 406.784c-70.203 49.356-170.226 44.736-235.638-20.676z"/></svg>
                    <svg v-if="item.status == 'pending' || item.status == 'cancel'"  @click="destroy(index, item.url, item.type)" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400" height="16px" width="16px"  xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License)<path d="M32 464a48 48 0 0 0 48 48h288a48 48 0 0 0 48-48V128H32zm272-256a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zm-96 0a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zm-96 0a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zM432 32H312l-9.4-18.7A24 24 0 0 0 281.1 0H166.8a23.72 23.72 0 0 0-21.4 13.3L136 32H16A16 16 0 0 0 0 48v32a16 16 0 0 0 16 16h416a16 16 0 0 0 16-16V48a16 16 0 0 0-16-16z"/></svg>

                </div>

                <img style="height: 100px !important; width: 100px !important;" width="100px" height="100px" class="upload-images object-cover " :src="item.url" >
                <circle-progress v-if="item.status == 'uploading'" :percent="item.percent" class="absolute "></circle-progress>

            </template >

            <!--      preview Videos     -->

            <template v-else-if="item.type == 'video'">
                <div  class="absolute h-full w-full bg-gray-100 rounded opacity-30" :class="(item.status == 'uploaded') && 'hidden'"></div>
                <div class="z-10 absolute tool-bar-img flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg " :class="(item.status == 'uploaded') && 'hidden'">
                    <svg v-if="item.status == 'uploading'" @click="cancelUpload(index)" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400  "  title="Cancel Upload" height="16px" width="16px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M256 8C119.034 8 8 119.033 8 256s111.034 248 248 248 248-111.034 248-248S392.967 8 256 8zm130.108 117.892c65.448 65.448 70 165.481 20.677 235.637L150.47 105.216c70.204-49.356 170.226-44.735 235.638 20.676zM125.892 386.108c-65.448-65.448-70-165.481-20.677-235.637L361.53 406.784c-70.203 49.356-170.226 44.736-235.638-20.676z"/></svg>
                    <svg v-if="item.status == 'pending' || item.status == 'cancel'"  @click="destroy(index, item.url, item.type)" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400" height="16px" width="16px"  xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License)<path d="M32 464a48 48 0 0 0 48 48h288a48 48 0 0 0 48-48V128H32zm272-256a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zm-96 0a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zm-96 0a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zM432 32H312l-9.4-18.7A24 24 0 0 0 281.1 0H166.8a23.72 23.72 0 0 0-21.4 13.3L136 32H16A16 16 0 0 0 0 48v32a16 16 0 0 0 16 16h416a16 16 0 0 0 16-16V48a16 16 0 0 0-16-16z"/></svg>

                </div>

                <video  class=" object-cover h-full"  autoplay loop muted playsinline>
                    <source :src="item.url" />
                </video>
                <circle-progress v-if="item.status == 'uploading'" :percent="item.percent" class="absolute "></circle-progress>
            </template>

            <!--      preview Files     -->
            <template v-else>
                <div  class="absolute h-full w-full bg-gray-100 rounded opacity-30" :class="(item.status == 'uploaded') && 'hidden'"></div>
                <div class="absolute tool-bar-img flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg " :class="(item.status == 'uploaded') && 'hidden'">
                    <svg v-if="item.status == 'uploading'" @click="cancelUpload(index)" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400  "  title="Cancel Upload" height="16px" width="16px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M256 8C119.034 8 8 119.033 8 256s111.034 248 248 248 248-111.034 248-248S392.967 8 256 8zm130.108 117.892c65.448 65.448 70 165.481 20.677 235.637L150.47 105.216c70.204-49.356 170.226-44.735 235.638 20.676zM125.892 386.108c-65.448-65.448-70-165.481-20.677-235.637L361.53 406.784c-70.203 49.356-170.226 44.736-235.638-20.676z"/></svg>
                    <svg v-if="item.status == 'pending' || item.status == 'cancel'"  @click="destroy(index, item.url, item.type)" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400" height="16px" width="16px"  xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512">Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License)<path d="M32 464a48 48 0 0 0 48 48h288a48 48 0 0 0 48-48V128H32zm272-256a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zm-96 0a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zm-96 0a16 16 0 0 1 32 0v224a16 16 0 0 1-32 0zM432 32H312l-9.4-18.7A24 24 0 0 0 281.1 0H166.8a23.72 23.72 0 0 0-21.4 13.3L136 32H16A16 16 0 0 0 0 48v32a16 16 0 0 0 16 16h416a16 16 0 0 0 16-16V48a16 16 0 0 0-16-16z"/></svg>

                </div>

                <img class="upload-images object-cover " :src="fileSrc" >
                <circle-progress v-if="item.status == 'uploading'" :percent="item.percent" class="absolute "></circle-progress>
            </template>

        </div>
    </section>
</template>

<script>
import CircleProgress from "./CircleProgress.vue";
import fileSrc from "./art/file.png";


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
