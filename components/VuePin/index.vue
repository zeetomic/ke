<template>
  <span class="payPassword">
    <input
      ref="input"
      :aria-disabled="disable"
      v-model="val"
      type="password"
      :name="name"
      class="sixDigitPassword"
      @keydown="keydown"
      @contextmenu="() => false"
      @paste="() => false"
      @copy="() => false"
      @cut="() => false"
      @blur="blur"
      autofocus="1"
      autocomplete="off"
      :maxlength="length"
      :minlength="length"
    />
    <div @focus="focus" class="sixDigitPassword" :tabindex="tabindex">
      <i :key="i" v-for="(index, i) in length">
        <b v-show="index <= val.length" />
      </i>
      <span v-show="status === MODE.FOCUS" :style="inputStyle" />
    </div>
  </span>
</template>

<script>
export default {
  name: "PayPasswrod",
  props: {
    onlyNumber: {
      type: Boolean,
      default: true
    },
    value: {
      type: String,
      default: ""
    },
    length: {
      type: Number,
      default: 4
    },
    tabindex: {
      type: Number,
      default: 0
    },
    disable: {
      type: Boolean,
      default: false
    },
    name: {
      type: String,
      default: "payPassword"
    }
  },
  data() {
    return {
      status: "",
      val: "",
      step: 0,
      MODE: {
        FOCUS: 1,
        BLUR: 2,
        COMPLETE: 3
      }
    };
  },
  computed: {
    inputStyle() {
      return {
        width: `40px`,
        left: `${this.val.length * this.step}px`
      };
    }
  },
  watch: {
    val(val) {
      this.$emit("input", val);
      if (this.status === this.MODE.BLUR) {
        return;
      }
      if (val.length === this.length) {
        this.status = this.MODE.COMPLETE;
      } else {
        this.status = this.MODE.FOCUS;
      }
    },
    value(value) {
      this.updateVal(value);
    }
  },
  mounted() {
    this.status = this.MODE.BLUR;
    this.updateVal(this.value);
    this.step = (this.$el.offsetWidth - 2) / this.length;
  },
  methods: {
    keydown(e) {
      if (!this.onlyNumber) {
        return;
      }
      const key = e.charCode || e.keyCode || 0;
      if (
        !(
          key == 8 ||
          key == 9 ||
          key == 46 ||
          key == 110 ||
          key == 190 ||
          (key >= 35 && key <= 40) ||
          (key >= 48 && key <= 57) ||
          (key >= 96 && key <= 105)
        )
      ) {
        e.preventDefault();
      }
    },
    updateVal(value) {
      if (this.val === value || value.length > this.length) {
        return;
      }
      this.val = value;
    },
    clear() {
      this.val = "";
      this.$emit("clear");
    },
    blur() {
      this.status = this.MODE.BLUR;
      this.$emit("blur");
    },
    focus(e) {
      if (this.disable) {
        if (e) {
          e.preventDefault();
          e.stopPropagation();
        }
        return false;
      }
      this.$refs.input.focus(e);
      if (this.val.length === this.length) {
        this.status = this.MODE.COMPLETE;
      } else {
        this.status = this.MODE.FOCUS;
      }
      this.$emit("focus");
    }
  }
};
</script>

<style scoped lang="less">
.payPassword {
  display: inline-block;
  *display: inline;
  zoom: 1;
  height: 24px;
  vertical-align: bottom;
  * {
    vertical-align: bottom;
  }
  input.sixDigitPassword {
    position: absolute;
    color: #fff;
    opacity: 0;
    width: 1px;
    height: 1px;
    font-size: 1px;
    left: 0;
    box-sizing: content-box;
    -webkit-user-select: initial;
    outline: none;
    margin-left: -9999px;
  }
  div.sixDigitPassword {
    cursor: text;
    background: #fafafa;
    outline: none;
    position: relative;
    padding: 8px 0;
    width: 190px;
    height: 20px;
    border: 1px solid #cccccc;
    border-radius: 2px;
    box-sizing: content-box;
    i {
      float: left;
      display: block;
      padding: 4px 0;
      height: 10px;
      width: 45px;
      border-left: 1px solid #cccccc;
      &:first-child {
        border-color: transparent;
      }
    }
    b {
      display: block;
      margin: 0 auto;
      width: 7px;
      height: 7px;
      overflow: hidden;
      background-image: url("../../assets/o.png");
    }
    span {
      position: absolute;
      display: block;
      left: 0px;
      top: 2px;
      height: 30px;
      border: 1px solid rgba(82, 168, 236, 0.8);
      border: ~"1px solid #00ffff\9";
      border-radius: 2px;
      box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.075),
        0 0 8px rgba(82, 168, 236, 0.6);
      background: url("../../assets/l.gif") no-repeat center center;
      background-size: auto 15px;
    }
  }
}
</style>