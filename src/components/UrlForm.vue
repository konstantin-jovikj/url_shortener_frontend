<script>
import axios from 'axios';

export default {
  data() {
    return {
      url: '',
      shortUrl: '',
      errorMessage: ''
    };
  },
  methods: {
    async shortenUrl() {
      try {
        const response = await axios.post('http://127.0.0.1:8000/api/generate_hash', { url: this.url });

        if (response.data.message === "URL is not safe") {
          var alertMessage = "URL is not safe.\n";
          alertMessage += "Threat Type: " + response.data.threat_type;

          alert(alertMessage);
        }

        if (response.data.message === 'URL already exists') {
          alert(response.data.message);
        } else {
          this.shortUrl = response.data.short_url_hash;
          console.log( this.shortUrl);
        }
      } catch (error) {
        console.error(error);
        alert('Error shortening URL');
      }
    }
  }
};
</script>

<template>

  <div class="border d-flex w-100">
    <h2 class="title">URL Shortener Form</h2>
    <form class="form d-flex" @submit.prevent="shortenUrl" >
      <input class="input" type="text" v-model="url" placeholder="Enter URL" required>
      <button type="submit">Shorten</button>
    </form>
    <div v-if="shortUrl" class="label">
      Short URL: <a :href="url" target="_blank">{{ shortUrl }}</a>
    </div>


  </div>
</template>

<style scoped>
  .border{
    border: 1px solid #007bff;
    border-radius: 30px;
    padding: 50px;
    filter: drop-shadow(8px 4px 9px #c2c2c2);
    background-color: white;
  }
.label{
  color: #850000;
  font-weight: bold;
  margin-top: 20px;
}
  .w-100{
    width: 100%;
  }
  .d-flex{
    display: flex;
    flex-direction: column;
  }
  .title{
    margin-bottom: 30px;
    text-align: center;

  }
  .form{
  width: 400px;
  }
  .input{
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


  button{
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
  button:hover{
    cursor: pointer;
    background-color: rgba(0, 68, 147, 0.89);
  }
</style>