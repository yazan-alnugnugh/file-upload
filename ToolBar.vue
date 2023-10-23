<template>
    <div class="tool-bar flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg ">
        
        <svg  @click="uploadAll()" class="fill-gray-400 transition-fill duration-700 cursor-pointer  hover:fill-green-500" height="17px" width="17px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M8 256C8 119 119 8 256 8s248 111 248 248-111 248-248 248S8 393 8 256zm143.6 28.9l72.4-75.5V392c0 13.3 10.7 24 24 24h16c13.3 0 24-10.7 24-24V209.4l72.4 75.5c9.3 9.7 24.8 9.9 34.3.4l10.9-11c9.4-9.4 9.4-24.6 0-33.9L273 107.7c-9.4-9.4-24.6-9.4-33.9 0L106.3 240.4c-9.4 9.4-9.4 24.6 0 33.9l10.9 11c9.6 9.5 25.1 9.3 34.4-.4z"/></svg>
        <svg @click="cancelUploadAll()" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400" height="17px" width="17px" xmlns="http://www.w3.org/2000/svg"  viewBox="0 0 512 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M256 8C119.034 8 8 119.033 8 256s111.034 248 248 248 248-111.034 248-248S392.967 8 256 8zm130.108 117.892c65.448 65.448 70 165.481 20.677 235.637L150.47 105.216c70.204-49.356 170.226-44.735 235.638 20.676zM125.892 386.108c-65.448-65.448-70-165.481-20.677-235.637L361.53 406.784c-70.203 49.356-170.226 44.736-235.638-20.676z"/></svg>
        <svg @click="deleteAll()" class="fill-gray-400 transition-fill	duration-700 cursor-pointer hover:fill-red-400" height="17px" width="17px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!-- Font Awesome Pro 5.15.4 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) --><path d="M268 416h24a12 12 0 0 0 12-12V188a12 12 0 0 0-12-12h-24a12 12 0 0 0-12 12v216a12 12 0 0 0 12 12zM432 80h-82.41l-34-56.7A48 48 0 0 0 274.41 0H173.59a48 48 0 0 0-41.16 23.3L98.41 80H16A16 16 0 0 0 0 96v16a16 16 0 0 0 16 16h16v336a48 48 0 0 0 48 48h288a48 48 0 0 0 48-48V128h16a16 16 0 0 0 16-16V96a16 16 0 0 0-16-16zM171.84 50.91A6 6 0 0 1 177 48h94a6 6 0 0 1 5.15 2.91L293.61 80H154.39zM368 464H80V128h288zm-212-48h24a12 12 0 0 0 12-12V188a12 12 0 0 0-12-12h-24a12 12 0 0 0-12 12v216a12 12 0 0 0 12 12z"/></svg>
       

    </div>
</template>

<script>
export default {
  methods:{
      uploadAll: function(){

          if(this.$parent.$data.uploading || this.$parent.$data.files.length == 0 || this.$parent.$data.files.every(thing => thing === null)) return;

              this.$parent.$data.uploading = true;

          this.$parent.$data.files.forEach((cur, index) =>{
              if(cur == null) return;

              this.$parent.$data.request.append('file', cur);
              this.$parent.$data.temporaryFiles[index].status = 'uploading';
              this.$parent.$data.temporaryFiles[index].source = this.$parent.createCancelToken();

              const config = {
                  headers: { 'Content-Type': 'multipart/form-data' },
                  cancelToken: this.$parent.$data.temporaryFiles[index].source.token,
                  onUploadProgress: progressEvent => {
                      const progress = (progressEvent.loaded / progressEvent.total) * 100;
                      this.$parent.$data.temporaryFiles[index].percent = progress;
                      if(progress === 100) {

                      }
                  }
              };

              axios.post(this.$parent.$props.url, this.$parent.$data.request, config)
                  .then(response => {
                      // handle success

                      this.$parent.$data.temporaryFiles[index].status = 'uploaded';
                      this.$parent.$data.files[index] = null;
                      if(this.$parent.$data.files.length - 1 == index) this.$parent.$data.uploading = false;
                      if(!this.$parent.isAllUploading()) this.$parent.$data.uploading = false;
                         this.emitter.emit('updateAll');
                  }).catch(function (error) {

                  console.log(error);

              });

              this.$parent.$data.request.delete('file');




          });


      },
      deleteAll : function(){

          this.cancelUploadAll();
          this.$parent.$data.temporaryFiles.forEach(cur => {
              URL.revokeObjectURL(cur);
          })
          this.$parent.$data.temporaryFiles = [];
          this.$parent.$data.files = [];
          this.$parent.$data.request.delete('file');

      },
      cancelUploadAll: function(){

              this.$parent.$data.uploading = false;

          this.$parent.$data.temporaryFiles.forEach(cur => {
              cur.source.cancel();
              cur.status = 'cancel';
          });

      },

  }
}
</script>
