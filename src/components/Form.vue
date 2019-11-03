<template>
  <div>
    <v-progress-linear v-show="showLoading" :indeterminate="true"></v-progress-linear>
    <v-card class="elevation-12">
      <v-card-text>
        <v-form>
          <v-flex>
            <v-textarea
              outlined
              no-resize
              v-model.trim="inputMessage"
              name="input-7-4"
              clearable
              label="messages.properties"
              hint="변환할 파일의 내용을 붙여넣기하세요"
              v-bind:readonly="isSuccess"
            ></v-textarea>
          </v-flex>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <div class="text-xs-center">

          <v-btn v-if="!isSuccess"
                 round
                 @click.prevent="onSubmitUrl"
                 color="primary"
                 large
                 dark>Convert
          </v-btn>
          <v-btn v-else
                 round
                 color="success"
                 large
                 v-clipboard:copy="inputMessage"
                 v-clipboard:success="onCopy"
                 dark>Copy
          </v-btn>
        </div>
      </v-card-actions>
    </v-card>
  </div>
</template>

<script>
import { mapMutations } from 'vuex'

export default {
  name: 'Form',
  data () {
    return {
      isSuccess: false,
      inputMessage: '',
      showLoading: false
    }
  },
  methods: {
    ...mapMutations([
      'SET_IS_SHOW_SNACKBER',
      'SET_IS_SHOW_ALERT',
    ]),
    onSubmitUrl () {
      if (!this.inputMessage) {
        this.SET_IS_SHOW_SNACKBER({ toggle: true, message: '내용을 입력해주세요' })
        return
      }

      this.toggleLoading()

      setTimeout(() => {

        let result = []
        this.inputMessage.split('\n').forEach((v, k) => {
          let key = v.split('=')[0], row = '';

          if (key.trim()) {
            if (key[0] === '#') {
              row = `//${key.replace(/#/gi, '')}`
            } else {
              row = `'${key}': /*[[#{${key}}]]*/,`
            }
          }

          result[k] = row
        })

        this.inputMessage = result.join('\n')

        this.isSuccess = true
        this.SET_IS_SHOW_ALERT({
          type: 'success',
          message: '변경이 완료되었습니다. COPY 버튼을 눌러서 주소를 복사해주세요.',
          toggle: true
        })

        this.toggleLoading()
      }, 300)

      // 실패처리
      // this.SET_IS_SHOW_ALERT({
      //   type: 'error',
      //   message: '변경이 실패되었습니다. 주소를 정확히 입력해주세요.',
      //   toggle: true
      // })
    },
    onCopy () {
      this.isSuccess = false
      this.inputMessage = ''
      this.SET_IS_SHOW_ALERT({
        type: 'info',
        message: '메시지가 복사되었습니다.(ctrl + v) ',
        toggle: true
      })
    },
    toggleLoading () {
      this.showLoading = !this.showLoading
    },
  }
}
</script>

<style scoped>

</style>
