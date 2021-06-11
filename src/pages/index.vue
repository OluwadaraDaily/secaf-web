<template>
  <div class="app-home">
    <div class="upload-section">
      <h1>Upload Image</h1>
      <form @submit.prevent="fileUpload">
        <input type="file" @change="handleChange" name="image" accept="image/*" id="imagefile">
        <p>
          <input type="submit" value="Upload Image" id="submit">
        </p>
      </form>
    </div> <hr>
    <h1>Gallery</h1>
    <div class="images-section">
      <div class="image" v-for="image in images" :key="image.id">
        <img
        :src="image['name']" height="100"
        >
        <button @click="deleteImage(image.id)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      imageData : null,
      images : []
    }
  },

  methods: {
    // Set the file data
    handleChange (e) {
      this.imageData = e.target.files[0]
      if(this.imageData != null) {
        document.getElementById("submit").disabled = false
      }
    },

    async getImages() {
      this.images = []
      const response = await this.$axios.$get("http://localhost:8000/get_images")
      this.images.push(...response['images'])
    },

    async fileUpload() {
      // Attach file to a form data type
      const formData = new FormData()
      formData.append('imageData',this.imageData)

      // Check to prevent null data POST request
      if (this.imageData != null) {
        const response = await this.$axios.$post('http://127.0.0.1:8000/upload', formData)
        // Send message depending on response
        if(response['success']) {
          // Show an alert response
          alert(response['success'] + "    " + "Faces: "  + response['faces'])

          // Set the value of the input file to null to prevent resubmission
          document.getElementById("imagefile").value = null

          // This is to persist the changes on the front end,
          // by getting the updated images list
          this.getImages()
        }
        else {
          alert(response['error'])
          document.getElementById("imagefile").value = null
        }
      }
      else {
        // Disable button if someone tries to submit an empty file
        document.getElementById("submit").disabled = true
      }

    },

    async deleteImage(id) {
      // The delete API call
      const response = await this.$axios.$delete(`http://localhost:8000/delete/${id}`)


      // This is to persist the changes on the front end,
      // by getting the updated images list
      this.getImages()

      // Alert a response
      alert(response['message'])
    }
  },

  mounted() {
    this.getImages()
  }
}
</script>

<style scoped>
.app-home {
  width: 95%;
  margin: 0 auto;
}
.upload-section {
  padding: 1rem 0;
}

input[type="file"] {
  width: 90%;
  padding: 3rem 1rem;
  background-color: #fff;
  color: black;
  border-radius: .35rem;
  border: 2px solid #e6f3f7;
}
input[type="submit"], .images-section button {
  border: 1px solid black;
  padding: .5rem 1rem;
  border-radius: .35rem;
  background-color: #fff;
}

input[type="submit"]:hover, .images-section button:hover {
  color: #e6f3f7;
  background-color: #000;
  cursor: pointer;
}

.images-section {
  display: flex;
  flex-wrap: wrap;
}
.image {
  padding: 1rem;
  margin: 2rem .5rem;
  display: flex;
  flex-direction: column;
  border: 1px solid #000;
  border-radius: .35rem;
}
.image > img {
  margin: .5rem 0;
}


</style>
