<template>
<body>
  <div class="container-fluid h-100">
      <div class="row h-100">
         <slide-bar></slide-bar>
          <div class="col-10" style="padding: 0;">
              <header-navar></header-navar>
            <div>
              <adminstrator-table></adminstrator-table>
            </div>
          </div>
      </div>
  </div>
</body>
</template>

<script>
import adminstratorTable from './AdminstratorTable.vue';
import SlideBar from './SlideBar.vue';
import HeaderNavar from './HeaderNavar.vue';

  export default {
    components: {
      adminstratorTable,
      SlideBar,
      HeaderNavar,
    },
    data () {
      return {
      }
    },
    methods: {

    },
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style type="text/css">
  html,
  body {
      height: 100%;
  }
  .imgRedonda {
    width:100%;
    height:100%;
    border-radius:150px;
  }

  .imgRedondaTable {
    width:50%;
    height:100%;
    border-radius:150px;
  }
  .help {
    margin-top: 90%;
    margin-left: 10%;
    margin-right: 10%;
    padding: 10px;
    background: rgb(232, 234, 240);
    opacity: 0.64;
    border-radius: 4px;
  }
  #navar {
    height: 100%;
    background-color: transparent;
    color: black;
    padding: 15px;
    align: center;
  }
  .breadcrumb {
    font-size: 14px;
    font-family: "Nunito", sans-serif;
    color: #899bbd;
    font-weight: 600;
  }
  a.link{
    font-family: 'Open Sans';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 24px;
    /* identical to box height, or 150% */
    letter-spacing: 0.1px;
  }
  a.secoundLink{
    font-family: 'Open Sans';
    font-style: normal;
    font-weight: 400;
    font-size: 16px;
    line-height: 24px;
    /* identical to box height, or 150% */
    letter-spacing: 0.1px;
    /* Secondary */
    color: #4FB9BB;
  }
  .menu li {
    margin-bottom: opx;
  }
  .menu li:hover {
    background:#E8EAF0;
  }

  .menu li a {
    box-sizing: border-box;
    display: flex;
    align-items: center;
    font-size: 15px;
    font-weight: 600;
    color: #4154f1;
    padding: 1% 0% 0% 10%;
    border-radius: 4px;
  }
</style>