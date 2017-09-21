<template>
  <div>
    <img class="image-preview" :src="image" />
    <input @change="onFileChange" type="file" />
    <button @click="removeImage">Remove image</button>
    <div id="allMetaDataSpan">

    </div>
  </div>
</template>


<script>
import EXIF from 'exif-js'

export default {
  props: ['image'],
  methods: {
    onFileChange (e) {
      var files = e.target.files || e.dataTransfer.files
      console.log(EXIF)
      if (!files.length) {
        return
      }
      EXIF.getData(files[0], function () {
        // debugger
        const allMetaData = EXIF.getAllTags(this)
        var allMetaDataSpan = document.getElementById('allMetaDataSpan')
        allMetaDataSpan.innerHTML = JSON.stringify(allMetaData, null, '\t')
      })
      this.createImage(files[0])
    },
    createImage (file) {
      var reader = new FileReader()
      reader.onload = (e) => {
        this.image = e.target.result
        this.$emit('change', this.image)
      }
      reader.readAsDataURL(file)
    },
    removeImage: function (e) {
      this.image = ''
    }
  }
}
</script>

<style>
.image-preview {
  
}
</style>
