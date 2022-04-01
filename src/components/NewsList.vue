<template>     
     <ul class="news__list"> 
         <li v-for="article in articles" class="news__item" v-bind:key="article in articles">
              <img v-bind:src= article.urlToImage />
              <div id="info">
                    <h5> {{ article.title }}</h5>
                    <p><i> {{ article.author}}</i></p>
                    <p> {{ article.description}}</p>
               </div>
        </li> 
                      
     </ul> 
    
</template> 

<script> 
    export default {   
        data() {     
            return {
                 articles: []
            };   
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

#info{
    padding: 15px;
   text-align: left;
}
</style>