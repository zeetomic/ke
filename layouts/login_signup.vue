<template>
  <nuxt />
</template>

<style lang="less">
*,
*:before,
*:after {
  margin: 0;
}
html {
  font-family: "Roboto Condensed", "Source Sans Pro", -apple-system,
    BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", Arial, sans-serif;
}
label.el-form-item__label {
  padding: 0!important;
}
.page-container {
  width: 100%;
  min-height: 100vh;
  // background: url("~assets/Zee_Background.jpg") center center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  background: #F0C27B;  /* fallback for old browsers */
  background: -webkit-linear-gradient(to right, #4B1248, #F0C27B);  /* Chrome 10-25, Safari 5.1-6 */
  background: linear-gradient(to right, #4B1248, #F0C27B); /* W3C, IE 10+/ Edge, Firefox 16+, Chrome 26+, Opera 12+, Safari 7+ */

  // background-color: #15151a;
  // background: linear-gradient(90deg, rgba(203,48,102,1) 36%, rgba(22,191,253,1) 79%); 
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover;
  filter: progid:DXImageTransform.Microsoft.AlphaImageLoader(src='.myBackground.jpg', sizingMethod='scale');
  -ms-filter: "progid:DXImageTransform.Microsoft.AlphaImageLoader(src='myBackground.jpg', sizingMethod='scale')";

  .gradient-color {
    background: linear-gradient(to right, #2bb8ca 0%, #89d074 40%);
    -webkit-background: linear-gradient(to right, #2bb8ca 0%, #89d074 40%);
    -webkit-text-fill-color: transparent;
    -webkit-background-clip: text;
    background-clip: text;
  }
  .Login,
  .Signup {
    padding: 10% 10%;
    h2 {
      padding-bottom: 2rem;
    }
  }
  a {
    color: #FF5733;
  }
  .login_register {
    width: 100%;
  }
  .zeetomic-footer {
    padding-top: 10rem;
    width: 150px;
  }
  .msg {
    color: red;
    padding-bottom: 1rem;
  }
}
@media only screen and (max-width: 768px) {
  .Login,
  .Signup {
    padding: 10% 10%;
    .el-form-item__error {
      position: initial;
    }
  }
}
</style>