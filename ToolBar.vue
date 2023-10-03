<template>
    <div class="tool-bar flex justify-content-start mb-6 text-gray-400 space-x-3 bg-gray-200 my-2 ml-2 bg-gray-100 w-max		p-2 rounded-lg ">
        <i @click="uploadAll()" title="Start Upload" class="transition-colors	duration-700 cursor-pointer  hover:text-green-500 fas fa-arrow-circle-up"></i>
        <i @click="cancelUploadAll()" title="Cancel Upload" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-ban"></i>
        <i @click="deleteAll()" title="Delete" class=" transition-colors	duration-700 cursor-pointer hover:text-red-400  fas fa-trash-alt"></i>

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
