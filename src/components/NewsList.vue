<template>  
  <form @submit.prevent="searchNews" class="d-flex flex-column justify-content-center">   
         <div class="input-group mx-sm-3 mb-2">         
             <label class="visually-hidden" for="search">Search </label>        
              <input type="search" name="search" v-model="searchTerm" id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />         
              <button class="btn btn-primary mb-2">Search</button>       
              </div>       
              <p>You are searching for {{ searchTerm }}</p>     
    </form> 

    <ul class="news__list"> 
         <li v-for="article in articles" class="news__item" v-bind:key="article in articles">
              <img v-bind:src= article.urlToImage />
              <div id="info">
                    <h5> {{ article.title }}</h5>
                    <p> {{ article.description}}</p>
               </div>
        </li>            
     </ul> 

</template> 

<script> 
    export default {   
        data() {     
            return {
                 articles: [],
                 searchTerm: ' '
            }  
            }, 
            methods: { 
                searchNews() {         
                    let self = this;         
                    fetch('https://newsapi.org/v2/everything?q='+ self.searchTerm + '&language=en', 
                    {     
                        headers: {'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`,} 
                        })           
                        .then(function(response) {             
                            return response.json();           
                            })           
                            .then(function(data) {            
                                 console.log(data);            
                                  self.articles = data.articles;          
                                   });      
                    }    
                 },
            created() {

                let self = this; 
                  fetch('https://newsapi.org/v2/top-headlines?country=us',
                 {
                      headers: {'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}` } 
                 })
                 .then(function(response) { 
                    return response.json();           
                 })           
                 .then(function(data) {             
                     console.log(data); 
                     self.articles = data.articles; 
                  }); 
            }
    }; 
</script> 

<style>
img{
    width: 300px;
    height:200px;
}

ul{
    list-style-type: none;
}

.news__list{
    padding-top: 20px;
    display: grid;
    grid-template-columns: repeat(3,300px);
    grid-gap: 50px;
}

.news__list li{
    border: 1px solid var(--black);
    box-shadow: 0 2px 5px 3px rgba(0, 0, 0, 0.1), 0 2px 4px -1px rgba(0, 0, 0, 0.06);
    width:300px;
    border-bottom: 10px solid green;
    border-bottom-left-radius: 5px;
    border-bottom-right-radius: 5px;  
}

p{
    text-align: left;
}
#info{
    padding: 15px;
    text-align: left;
}
</style>
