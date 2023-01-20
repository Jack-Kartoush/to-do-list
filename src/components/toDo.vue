
<template>
    <div class="warning"> 
    <span>You have to fil the filed</span>
    </div>
  </template>

  <script>
   export default{
    name: 'toDo'
   }
</script>
  <style>
    .warning {
     
     display: flex;
     justify-content: center;
     align-items: center;
     margin-bottom: 10px;
    
    }
    .warning span{
      text-align: center;
      padding: 15px;
      border-radius: 40px 0 20px 0;
      background-color: rgb(200, 8, 8);
      color:white;
      font-weight: bold;
    }
  </style>
 

