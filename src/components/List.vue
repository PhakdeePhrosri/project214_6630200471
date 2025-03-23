<template>
  <ul class="list-group" style="width: 30rem">
      <!--ทำเป็นตาราง-->
      <li v-for="(std, id) in students" :key="id" class="list-group-item">
        <!--วนลูปใส่ค่า-->
        <a href="#" @click="std.isActive=!std.isActive">
        {{ name.Name }} {{ name.ID }} {{ name.NO }}  {{ name.School }}
        </a>
        <!--ค่าที่จะใส่-->
        <div class="mt-3" v-if="name.isActive">
          <StdDetail :nameID=name.id :nameName="name.Name" :nameNO="name.NO" :nameSchool="name.School"/>
        </div>
      </li>
    </ul>
</template>

<script> 
import { EventBus } from '../event-bus';
import Detail from "./Detail.vue";
export default {
    name : "List",
    components: {
        NameDetail,
    },
    data() {
        return {
            Detail:[],
            showlist : false
        }
    },
    mounted(){
      fetch('http://localhost:3000/Default')
      .then(res=>res.json())
      .then(data=>this.Default=data)
      .catch(err=>console.log(err.message))
    },
    mounted(){
      this.getData()

      EventBus.on('nameChange',(data)=>{
        console.log(`Info has change. -->${data.meesage}`)
        this.getData()
    })
    },
    methods:{
      getData(){
        fetch(`http://localhost:3000/Default`)
        .then(res=>res.json())
        .then(data=>this.Default =data)
        .catch(err=>console.error(err.message))
      }
    }
}
</script>

<style>

</style>