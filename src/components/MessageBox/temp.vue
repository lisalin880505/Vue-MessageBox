<template>
  <div class="mint-msgbox-wrapper">
    <transition name="msgbox-bounce">
      <div class="mint-msgbox" v-show="value" :class="[ boxClass ]">
        <div class="mint-msgbox-header" v-if="title !== ''">
          <div class="mint-msgbox-title">{{ title }}</div>
        </div>
        <div class="mint-msgbox-content" v-if="message !== ''">
          <div class="mint-msgbox-message" v-html="message"></div>
          <div class="mint-msgbox-input" v-show="showInput">
            <input v-model="inputValue" :placeholder="inputPlaceholder" ref="input">
            <div class="mint-msgbox-errormsg" :style="{ visibility: !!editorErrorMessage ? 'visible' : 'hidden' }">{{ editorErrorMessage }}</div>
          </div>
        </div>
        <div class="mint-msgbox-btns" v-if="showCancelButton || showConfirmButton">
          <button :class="[ cancelButtonClasses ]" v-if="showCancelButton" @click="handleAction('cancel')">{{ cancelButtonText }}</button>
          <button :class="[ confirmButtonClasses ]" v-if="showConfirmButton" @click="handleAction('confirm')">{{ confirmButtonText }}</button>
        </div>
      </div>
    </transition>
  </div>
</template>

<style rel="stylesheet/scss" lang="scss">
  .v-modal-enter {
    animation: v-modal-in .2s ease;
  }
  .v-modal-leave {
    animation: v-modal-out .2s ease forwards;
  }
  @keyframes v-modal-in {
    0% { opacity: 0;}
    100% {}
  }
  @keyframes v-modal-out {
    0% {}
    100% { opacity: 0;}
  }
  .v-modal {
    position: fixed;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    opacity: 0.4;
    background: #000;
  }

</style>
<style rel="stylesheet/scss" lang="scss" scoped>
  .mint-msgbox-wrapper{
    font-family: -apple-system, BlinkMacSystemFont, "PingFang SC", "Helvetica Neue", STHeiti, "Microsoft Yahei", Tahoma, Simsun, sans-serif;
    -webkit-user-select: none;
    -webkit-tap-highlight-color: rgba(255, 255, 255, 0);
  }

  .mint-msgbox {
    transition: 0.5s;
    position: fixed;
    z-index: 999999;
    top: 50%;
    left: 50%;
    overflow: hidden;
    width: 270px;
    -webkit-transform: translate3d(-50%, -50%, 0) scale(1);
    text-align: center;
    color: #000;
    border-radius: 13px;
    background: rgba(255, 255, 255, .95);

    &.mint-msgbox-toast {
      border-radius: 5px;

      .mint-msgbox-content{
        color: #fff;
        background-color: rgba(0, 0, 0, 0.85);
      }
    }

    &.msgbox-bounce-enter {
      display: block;
      transform: translate3d(-50%, -50%, 0) scale(1.185);
      opacity: 1;
    }
    &.msgbox-bounce-leave-active {
      transform: translate3d(-50%, -50%, 0) scale(1);
      opacity: 0;
    }

    .mint-msgbox-header {
      padding: 15px 0 0;
      border-radius: 13px 13px 0 0;
      background: rgba(255, 255, 255, .95);
    }
    .mint-msgbox-content {
      position: relative;
      padding-top: 5px;
      background: rgba(255, 255, 255, .95);

      &:after {
        position: absolute;
        z-index: 15;
        top: auto;
        right: auto;
        bottom: 0;
        left: 0;
        display: block;
        width: 100%;
        height: 1px;
        content: '';
        transform: scaleY(.5);
        transform-origin: 50% 100%;
        background-color: rgba(0, 0, 0, .2);
      }
    }
    .mint-msgbox-input {
      & input {
        border: 1px solid #dedede;
        border-radius: 5px;
        padding: 4px 5px;
        width: 90%;
        appearance: none;
        outline: none;
        box-sizing: border-box;
      }
      & input.invalid {
        border-color: #ff4949;
        &:focus {
          border-color: #ff4949;
        }
      }
    }
    .mint-msgbox-errormsg {
      color: red;
      font-size: 12px;
      min-height: 18px;
      margin-top: 2px;
    }
    .mint-msgbox-title {
      font-size: 18px;
      font-weight: 500;
      text-align: center;
    }
    .mint-msgbox-message {
      font-family: inherit;
      font-size: 14px;
      padding: 5px 10px 15px 10px;
      line-height: 1.4;
    }
    .mint-msgbox-btns {
      position: relative;
      display: flex;
      height: 44px;
      justify-content: center;
    }
    .mint-msgbox-btn {
      font-size: 17px;
      line-height: 44px;
      position: relative;
      display: block;
      overflow: hidden;
      box-sizing: border-box;
      width: 100%;
      height: 44px;
      padding: 0 5px;
      text-align: center;
      white-space: nowrap;
      text-overflow: ellipsis;
      color: #007aff;
      background: rgba(255, 255, 255, .95);
      letter-spacing: 0.5px;
      cursor: pointer;
      border: none;
      flex-shrink: 1;
      flex-grow: 1;
      &:first-child {
        border-bottom-left-radius: 13px;
      }
      &:first-child:last-child {
        border-radius: 0 0 13px 13px;
      }
      &:last-child {
        border-bottom-right-radius: 13px;

        &:before{
          position: absolute;
          z-index: 15;
          top: 0;
          right: 0;
          left: 0;
          bottom: auto;
          display: block;
          width: 1px;
          height: 100%;
          content: '';
          transform: scaleX(.5);
          transform-origin: 100% 50%;
          background-color: rgba(0, 0, 0, .2);
        }
      }
      &:last-child:after {
        display: none;
      }
      &:focus {
        outline: none;
      }
      &:active {
        background-color: #fff;
      }
    }
  }
</style>

<script type="text/babel">
/* eslint-disable */
  let CONFIRM_TEXT = '确定';
  let CANCEL_TEXT = '取消';
  import Popup from './popup';
  export default {
    mixins: [ Popup ],
    props: {
      modal: {
        default: true
      },
      showClose: {
        type: Boolean,
        default: true
      },
      lockScroll: {
        type: Boolean,
        default: false
      },
      closeOnClickModal: {
        default: true
      },
      closeOnPressEscape: {
        default: true
      },
      inputType: {
        type: String,
        default: 'text'
      }
    },
    computed: {
      boxClass() {
        return 'mint-msgbox-' + this.type
      },
      confirmButtonClasses() {
        let classes = 'mint-msgbox-btn mint-msgbox-confirm ' + this.confirmButtonClass;
        if (this.confirmButtonHighlight) {
          classes += ' mint-msgbox-confirm-highlight';
        }
        return classes;
      },
      cancelButtonClasses() {
        let classes = 'mint-msgbox-btn mint-msgbox-cancel ' + this.cancelButtonClass;
        if (this.cancelButtonHighlight) {
          classes += ' mint-msgbox-cancel-highlight';
        }
        return classes;
      }
    },
    methods: {
      doClose() {
        this.value = false;
        this._closing = true;
        this.onClose && this.onClose();
        setTimeout(() => {
          if (this.modal && this.bodyOverflow !== 'hidden') {
            document.body.style.overflow = this.bodyOverflow;
            document.body.style.paddingRight = this.bodyPaddingRight;
          }
          this.bodyOverflow = null;
          this.bodyPaddingRight = null;
        }, 200);
        this.opened = false;
        if (!this.transition) {
          this.doAfterClose();
        }
      },
      onOpen: function () {
        if (this.$type === 'toast' && this.toastDuration != null){
          setTimeout(() => {
            this.close()
            this.callback && this.callback()
          }, this.toastDuration)
        }
      },
      handleAction(action) {
        if (this.$type === 'prompt' && action === 'confirm' && !this.validate()) {
          return;
        }

        var callback = this.callback;
        this.value = false;
        callback(action);
      },
      validate() {
        if (this.$type === 'prompt') {
          var inputPattern = this.inputPattern;
          if (inputPattern && !inputPattern.test(this.inputValue || '')) {
            this.editorErrorMessage = this.inputErrorMessage || '输入的数据不合法!';
            this.$refs.input.classList.add('invalid');
            return false;
          }
          var inputValidator = this.inputValidator;
          if (typeof inputValidator === 'function') {
            var validateResult = inputValidator(this.inputValue);
            if (validateResult === false) {
              this.editorErrorMessage = this.inputErrorMessage || '输入的数据不合法!';
              this.$refs.input.classList.add('invalid');
              return false;
            }
            if (typeof validateResult === 'string') {
              this.editorErrorMessage = validateResult;
              return false;
            }
          }
        }
        this.editorErrorMessage = '';
        this.$refs.input.classList.remove('invalid');
        return true;
      },
      handleInputType(val) {
        if (val === 'range' || !this.$refs.input) return;
        this.$refs.input.type = val;
      }
    },
    watch: {
      inputValue() {
        if (this.$type === 'prompt') {
          this.validate();
        }
      },
      value(val) {
        this.handleInputType(this.inputType);
        if (val && this.$type === 'prompt') {
          setTimeout(() => {
            if (this.$refs.input) {
              this.$refs.input.focus();
            }
          }, 500);
        }
      },
      inputType(val) {
        this.handleInputType(val);
      }
    },
    data() {
      return {
        title: '',
        message: '',
        type: '',
        showInput: false,
        inputValue: null,
        inputPlaceholder: '',
        inputPattern: null,
        inputValidator: null,
        inputErrorMessage: '',
        showConfirmButton: true,
        showCancelButton: false,
        confirmButtonText: CONFIRM_TEXT,
        cancelButtonText: CANCEL_TEXT,
        confirmButtonClass: '',
        confirmButtonDisabled: false,
        cancelButtonClass: '',
        editorErrorMessage: null,
        toastDuration: 3000,
        callback: null
      };
    }
  };
</script>
