<script setup>
    import { Configuration, OpenAIApi } from "openai";
</script>


<template>
  <div class="greetings">    
    <h3>
      <div class="grid-container">
        <div class="grid-item">
          <p> Number of Days</p>
          <input
            v-model="days"
            type="text"
            placeholder="Days for plan" 
          />
        </div>
        <div class="grid-item">
          <p> Dietary Restrictions</p>
          <input
            v-model="dietR"
            type="text"
            placeholder="Regular, Vegitarian etc..." 
          />
        </div>
        <div class="grid-item">
          <p> Days to Exclude</p>
          <input
            v-model="dte"
            type="text"
            placeholder="Days to exclude" 
          />
        </div>  
        <div class="grid-item">
          <p> Other Notes</p>
          <input
            v-model="other"
            type="text"
            placeholder="I want tacos" 
          />
        </div>
        
      </div>
      <button v-on:click="apply()"> Create Dinner Plan! </button>
      <div class="textFeild pre-formatted" contenteditable="true">
        {{ msg }}    
      </div>
    </h3>
  </div>
</template>

<script>
export default {
    data(){
        return {
            msg: "Your Dinner Plan will appear here! Your Dinner Plan will appear here! Your Dinner Plan will appear here! Your Dinner Plan will appear here! ",
            days: "",
            dietR: "",
            dte: "",
            other: "",
        }
    },
    methods:{
        async apply(){
            const configuration = new Configuration({
                apiKey: import.meta.env.VITE_OPENAI_API_KEY,
                organization: import.meta.env.VITE_ORG_ID,
            });

            delete configuration.baseOptions.headers['User-Agent'];

            const openai = new OpenAIApi(configuration);
            
            const completion = await openai.createChatCompletion({
                model: "gpt-3.5-turbo",
                messages: [{"role": "system", "content": "You are a helpful dinnerplan creation tool."}, 
                {"role": "user", "content": "Create a" + this.days + "day dinner plan that emphasizes minimal ingredient usage and incorporates leftovers for efficiency. It should not contain instructions and it should only contain the list and no other text" + 
                  "I have the following contraints: " +
                  this.dietR + this.getDTE()}]
            });
            this.msg = completion.data.choices[0].message.content
        },
        getDTE(){
          if(this.dte === ""){
            return "";
          } else{
            return " Days " + this.dte + " are to be excluded. "
          }
        }
    },
    
}
</script>

<style scoped>

.grid-container {
  display: grid;
  grid-template-columns: auto auto;
  padding: 10px;
}
.grid-item {
  padding: 20px;
  text-align: center;
}

.textFeild{
    background-color: aliceblue;
    color:black;
    border-radius: 1em;
    padding: 15px;
    min-width: 800px;
    max-width: 800px;
    min-height: 300px;
    margin: auto;
    margin-top: 30px;

}

.pre-formatted {
  white-space: pre-wrap;
}

h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

</style>
