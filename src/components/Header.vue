<template>
    <div>
         <b-nav>
    <b-nav-item  disabled>Simple Vue Quiz</b-nav-item>
    <b-nav-item disabled>Counter {{ nCorrect}}/{{nTotal}}</b-nav-item>

  </b-nav>
    </div>
</template>

<script>
export default {
  props:[
    'nCorrect',
    'nTotal'
  ]
}
</script>