<template>
  <div class="border d-flex w-100">
    <h2 class="title">URL Shortener Form</h2>
    <form class="form d-flex" @submit.prevent="shortenUrl">
      <input class="input" type="text" v-model="url" placeholder="Enter URL" required />
      <button type="submit">Shorten</button>
    </form>
    <div v-if="shortUrl" class="label">
      Short URL: <a :href="url" target="_blank">{{ shortUrl }}</a>
    </div>
    <div v-if="errorMessage" class="error-message">
      {{ errorMessage }}
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      url: '',
      shortUrl: '',
      errorMessage: ''
    }
  },
  methods: {
    async shortenUrl() {
      try {
        // Fetch data from the backend endpoint
        const response = await axios.post('http://127.0.0.1:8000/api/generate_hash', {
          url: this.url
        })

        this.handleResponse(response.data)
      } catch (error) {
        this.errorMessage = 'An error ocured. PLease enter a valid URL.'
      }
    },
    //Handle the response data object based on the value of the property 'message' of the data object
    handleResponse(data) {
      if (data.message === 'URL is not safe') {
        this.alertUnsafeUrl(data)
      } else if (data.message === 'URL already exists') {
        this.errorMessage = 'This URL already exists in the database.'
      } else {
        this.shortUrl = data.short_url_hash
        this.errorMessage = ''
      }
    },
    //this method handles the response if the  message property in data object equals 'URL is not safe' and shows the threat type
    alertUnsafeUrl(data) {
      let alertMessage = 'URL is not safe.\n'
      alertMessage += `Threat Type: ${data.threat_type}`
      alert(alertMessage)
    }
  }
}
</script>

<style scoped>
.border {
  border: 1px solid #007bff;
  border-radius: 30px;
  padding: 50px;
  filter: drop-shadow(8px 4px 9px #c2c2c2);
  background-color: white;
}
.label,
.error-message {
  margin-top: 20px;
}
.label {
  color: #850000;
  font-weight: bold;
}
.error-message {
  color: #ff0800;
}
.w-100 {
  width: 100%;
}
.d-flex {
  display: flex;
  flex-direction: column;
}
.title {
  margin-bottom: 30px;
  text-align: center;
}
.form {
  width: 400px;
}
.input {
  display: block;
  padding: 0.75rem 1rem;
  font-size: 1rem;
  font-weight: 400;
  line-height: 1.5;
  color: #495057;
  background-color: #fff;
  background-clip: padding-box;
  border: 1px solid #ced4da;
  border-radius: 0.25rem;
}

.input:focus {
  color: #495057;
  background-color: #fff;
  border-color: #80bdff;
  outline: 0;
  box-shadow: 0 0 0 0.2rem rgba(0, 123, 255, 0.25);
}

button {
  margin-top: 25px;
  padding: 10px 20px;
  background-color: #007bff;
  border: none;
  border-radius: 0.25rem;
  color: white;
  font-size: 1rem;
  font-weight: 400;
  text-align: center;
  vertical-align: middle;
  cursor: pointer;
}
button:hover {
  background-color: rgba(0, 68, 147, 0.89);
}
</style>
