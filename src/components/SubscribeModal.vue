<template>
  <div class="subscribe-modal">
    <i @click="$emit('hideModal')" class="hoverable cancel fas fa-times-circle"></i>
    <h3><strong>Subscribe!</strong></h3>
    <p class="invitation-text">Consider <strong>subscribing</strong> to our mailing list.</p>
    <!--<p v-if="errorMessage" class="invitation-text">{{errorMessage}}</p>-->
    <input v-model='emailAddress'
           @keyup.enter="send()"
           class="email"
           type="text"
           placeholder="your.email@address.com">
    <button @click="send()">{{ errorMessage ? errorMessage : defaultMessage }}</button>
    <div class="checkbox">
      <input v-model='agreedToTerms' type="checkbox"> I agree to <a class="hoverable"
                                                                    target="_blank"
                                                                    href="https://www.youtube.com/watch?v=YS4DW8XLclE">terms of use</a>  <!--TODO: remove papiesz-->

    </div>
  </div>
</template>

<script>
  import vaildEmail from '../emailValidation'
  let defaultMessage = 'I\'m in!';


  export default {
    name: "SubscribeModal",
    data: function() {
      return {
        agreedToTerms: false,
        emailAddress: '',
        errorMessage: '',
        defaultMessage: defaultMessage
      }
    },
    methods: {
      send: function() {
        if (vaildEmail(this.emailAddress) && this.agreedToTerms) {
          console.log(`sent:
          address: ${this.emailAddress}
          agreement: ${this.agreedToTerms}`);
          this.errorMessage = 'Thanks!';

          setTimeout(() => {
            this.$emit('hideModal')
          }, 800);

          return
        }

        if (!this.agreedToTerms) {
          this.errorMessage = 'Must agree to terms of use\n';
        }

        if (!vaildEmail(this.emailAddress)) {
          this.errorMessage = 'Invalid email address!\n';
        }

        setTimeout(() => {
          this.errorMessage = '';
        }, 3500)
      }
    }
  }
</script>

<style scoped lang="scss">
  .cancel {
    grid-row: 1;
    grid-column: 3;
    align-self: start;
    justify-self: end;
    font-size: 2.8rem;
    color: lightcoral;
  }

  .hoverable {
    cursor: pointer;
    color: lightcoral;
    &:hover {
      opacity: 0.75;
    }
  }
  strong {
    color: lightcoral;
    cursor: pointer;
    &:hover {
      opacity: 0.75;
    }
  }
  .subscribe-modal {
    max-width: 90%;
    max-height: 75%;
    right: 0;
    left: 0;
    margin-top: 10%;
    margin-left: auto;
    margin-right: auto;
    position: absolute;
    width: 600px;
    height: 600px;
    z-index: 1;
    background-color: rgba(255, 243, 205, 0.94);
    border-radius: 1rem;
    display: grid;
    grid-template-rows: 4fr 2fr 2fr 2fr 2fr;
    grid-template-columns: 1fr 10fr 1fr;

    border: 6px solid lightcoral;
    box-shadow: 5px 10px 2000px lightcoral;

    font-size: 1.5rem;

    /*grid*/
    align-items: center;
    justify-items: center;

    h3 {
      grid-row: 1;
      grid-column: 2;
      font-weight: 900;
      font-size: 3rem;
      color: coral;
    }
    .invitation-text {
      grid-row: 2;
      grid-column: 2;
    }

    .email {
      grid-row: 3;
      grid-column: 2;
      width: 100%;
      height: 80%;
      min-height: 80%;
      line-height: 1.5rem;
      text-align: center;
      font-size: 1.5rem;
      border-radius: 0.5rem
    }


    button {
      grid-row: 4;
      grid-column: 2;

      width: 102%;
      height: 85%;
      border: 0.4rem solid lightcoral;
      font-family: inherit;
      font-size: inherit;
      line-height: 1.76rem;
      color: whitesmoke;
      background-color: lightcoral;
      cursor: pointer;
      padding: 25px 80px;
      display: inline-block;
      text-transform: uppercase;
      letter-spacing: 1px;
      font-weight: 900;
      outline: none;
      position: relative;
      -webkit-transition: all 0.3s;
      -moz-transition: all 0.3s;
      transition: all 0.3s;
      border-radius: 0.5rem;

      &:hover {
        background-color: whitesmoke;
        color: lightcoral;
      }
    }

    .checkbox {
      grid-row: 5;
      grid-column: 2;
      input {
        position: relative;
        top: 3px;
      }
    }
  }

</style>
