<template>
  <div class="container">
    <div class="demo">
      <text class="value-text"></text>
      <wxc-searchbar ref="wxc-searchbar"
                     @wxcSearchbarCancelClicked="wxcSearchbarCancelClicked"
                     @wxcSearchbarInputReturned="wxcSearchbarInputReturned"
                     @wxcSearchbarInputOnInput="wxcSearchbarInputOnInput"
                     @wxcSearchbarCloseClicked="wxcSearchbarCloseClicked"
                     @wxcSearchbarInputOnFocus="wxcSearchbarInputOnFocus"
                     @wxcSearchbarInputOnBlur="wxcSearchbarInputOnBlur"></wxc-searchbar>
      <text class="value-text">{{value}}</text>
    </div>
  </div>
  
</template>

<style scoped>

</style>

<script>
  import { WxcSearchbar } from 'weex-ui'
  export default {
    components: { WxcSearchbar },
    data: () => ({
      value: '',
    }),
    methods: {
      wxcSearchbarInputOnFocus () {
      },
      wxcSearchbarInputOnBlur () {
      },
      wxcSearchbarInputReturned (){
      },
      wxcSearchbarCloseClicked () {
      },
      wxcSearchbarInputOnInput (e) {
        this.value = e.value;
      },
      wxcSearchbarCancelClicked () {
      },
      wxcSearchbarInputDisabledClicked () {
      },
      wxcSearchbarDepChooseClicked () {
      }
    },
  };
</script>
