<template>
  <div id="background">
  <div class="container-fluid d-flex align-items-center justify-content-between fixed-top bg-light">
    <img src="/logo.png" class="img-fluid" alt="Logo">
    <h3 id="name">Hexa Shortner</h3>
    <a href="https://github.com/RubensRafael/HexaShortner" target="_blank">
      <img src="/github.svg" class="img-fluid" alt="Github Logo">
    </a>
  </div>
    <form  @submit="handleSubmit" class="container-fluid d-flex align-items-center justify-content-center gap-3">
      <div v-if="success">
        <a class="badge rounded-pill bg-success" target="_blank" v-bind:href="success">{{success}}</a>
      </div>
      <div v-if="warn">
        <span class="badge rounded-pill bg-danger">{{warn}}</span>
      </div>
     
      <div id="input-container" class="p-2 rounded-pill" >
        <input :disabled="loading" v-model="url" placeholder="https://www.google.com" type="text" name="url">
      </div>
      <button class="rounded-circle" type="submit">
        <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-send" viewBox="0 0 16 16">
        <path d="M15.854.146a.5.5 0 0 1 .11.54l-5.819 14.547a.75.75 0 0 1-1.329.124l-3.178-4.995L.643 7.184a.75.75 0 0 1 .124-1.33L15.314.037a.5.5 0 0 1 .54.11ZM6.636 10.07l2.761 4.338L14.13 2.576 6.636 10.07Zm6.787-8.201L1.591 6.602l4.339 2.76 7.494-7.493Z"/>
        </svg>
      </button>
    </form>
  </div>
</template>

<style type="text/css">
  *{
    border: 0;
    padding: 0;
    box-sizing: border-box;
  }
  #name{
    color: #4db351;
  }
  #background{
    height: 100vh;
    width: 100%;
    display: flex;
    justify-content: center;
    background-color: #4db351;
  }
  form{
    flex-wrap: wrap;
  }
  #input-container{
    background-color: white;
  }
  input{
    border: none;
    outline: none;
  }
  button {
    display: block;
    background-color: white;
    height: 40px;
    width: 40px;
}

</style>
<script>
export default {
  data() {
    return {
      url: '',
      history: [],
      warn: '',
      success:'',
      loading: false
    }
  },
  methods:{
    handleSubmit : function(e){
      
      e.preventDefault()

      this.loading = true
      this.warn = ''
      this.success = ''
      fetch('https://hexa-shortner-api.onrender.com/',{
        method:'POST',
        body:JSON.stringify({"url":this.url}),
        headers: {
        'Content-Type': 'application/json'
      }
      }).then(async(res)=>{
        if(res.status === 400){
          this.warn = "Digite um URL válida"
        }else{
          const result = await res.json().then((data)=>data.result)
          this.success = "https://hxshrt.tk/" + result
        }
      })
      this.loading = false
    }
  },
  
}
</script>
