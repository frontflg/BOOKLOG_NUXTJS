<template>
  <div>
    <v-container>
      <v-snackbar
        v-model="alertFlg"
        color="red"
        style="white-space: pre-wrap;"
        :top="true"
      >
        {{ alertMessage }}
        <v-btn
          color="white"
          text
          @click="alertFlg = false"
        >
          Close
        </v-btn>
      </v-snackbar>
    </v-container>
    <v-row>
      <v-col cols="2">
        <v-card-title
          outlined
        >
          BOOK
        </v-card-title>
      </v-col>
      <v-col cols="1" align-self="center">
        <v-btn
          x-small
          class="primary"
          @click="closeWindow()"
        >
          ISBN13
        </v-btn>
      </v-col>
      <v-col cols="2" align-self="center">
        {{ book[0].ISBN13 }}
      </v-col>
      <v-col cols="1" align-self="center">
        <v-btn
          x-small
          class="primary"
          @click="openAmazon()"
        >
          ISBN10
        </v-btn>
      </v-col>
      <v-col cols="2" align-self="center">
        <v-text-field
          v-model.number="inIsbn10"
          background-color="white"
          clearable
          maxlength="10"
        />
      </v-col>
      <v-col cols="4">
        <v-text-field
          v-model="inCoverImg"
          label="表紙："
          background-color="white"
          clearable
          maxlength="25"
        />
      </v-col>
    </v-row>
    <v-row>
      <v-col cols="8">
        <v-row no-gutters>
          <v-col>
            <v-text-field
              v-model="inBookName"
              label="書名："
              background-color="white"
              clearable
              maxlength="50"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="6">
            <v-text-field
              v-model="inAuthor"
              label="著者："
              background-color="white"
              clearable
              maxlength="25"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="inGenre"
              label="分類："
              background-color="white"
              clearable
              maxlength="25"
            />
          </v-col>
          <v-col cols="2">
            <v-text-field
              v-model="inState"
              label="状況："
              background-color="white"
              clearable
              maxlength="10"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col cols="4">
            <v-text-field
              v-model="inPublisher"
              label="出版社："
              background-color="white"
              clearable
              maxlength="25"
            />
          </v-col>
          <v-col cols="2">
            <v-select
              v-model="select"
              label="所有："
              :items="items"
              background-color="white"
            />
          </v-col>
          <v-col cols="4">
            <v-text-field
              v-model="inLibrary"
              label="図書館・書店："
              background-color="white"
              clearable
              maxlength="25"
            />
          </v-col>
          <v-col cols="2">
            <v-text-field
              v-model.number="inPurchase"
              label="価格(円)："
              background-color="white"
              clearable
              maxlength="11"
            />
          </v-col>
        </v-row>
        <v-row no-gutters>
          <v-col>
            <v-menu
              ref="menu1"
              v-model="menu1"
              :close-on-content-click="false"
              :return-value.sync="inIssueDate"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template #activator="{ on, attrs }">
                <v-text-field
                  v-model="inIssueDate"
                  label="発行日："
                  clearable
                  prepend-icon="mdi-calendar"
                  background-color="white"
                  v-bind="attrs"
                  v-on="on"
                />
              </template>
              <v-date-picker
                v-model="inIssueDate"
                scrollable
                color="blue"
                locale="ja-jp"
                :day-format="date => new Date(date).getDate()"
                @input="
                  $refs.menu1.save(inIssueDate)
                  menu1 = false
                "
              />
            </v-menu>
          </v-col>
          <v-col>
            <v-menu
              ref="menu2"
              v-model="menu2"
              :close-on-content-click="false"
              :return-value.sync="inGetDate"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template #activator="{ on, attrs }">
                <v-text-field
                  v-model="inGetDate"
                  label="入手日："
                  clearable
                  prepend-icon="mdi-calendar"
                  background-color="white"
                  v-bind="attrs"
                  v-on="on"
                />
              </template>
              <v-date-picker
                v-model="inGetDate"
                scrollable
                color="blue"
                locale="ja-jp"
                :day-format="date => new Date(date).getDate()"
                @input="
                  $refs.menu2.save(inGetDate)
                  menu2 = false
                "
              />
            </v-menu>
          </v-col>
          <v-col>
            <v-menu
              ref="menu3"
              v-model="menu3"
              :close-on-content-click="false"
              :return-value.sync="inReadDate"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template #activator="{ on, attrs }">
                <v-text-field
                  v-model="inReadDate"
                  label="読了日："
                  clearable
                  prepend-icon="mdi-calendar"
                  background-color="white"
                  v-bind="attrs"
                  v-on="on"
                />
              </template>
              <v-date-picker
                v-model="inReadDate"
                scrollable
                color="blue"
                locale="ja-jp"
                :day-format="date => new Date(date).getDate()"
                @input="
                  $refs.menu3.save(inReadDate)
                  menu3 = false
                "
              />
            </v-menu>
          </v-col>
        </v-row>
        <v-row no-gutters>
          <v-col>
            <v-text-field
              v-model="inOverview"
              label="概要："
              background-color="white"
              clearable
              maxlength="255"
            />
          </v-col>
        </v-row>
        <v-row no-gutters>
          <v-col>
            <v-textarea
              v-model="inImpressions"
              label="感想："
              background-color="white"
              clearable
              auto-grow
              counter
            />
          </v-col>
        </v-row>
      </v-col>
      <v-col cols="4">
        <v-row>
          <v-col>
            <v-img
              max-height="550"
              max-width="350"
              :src="`${book[0].CoverImg}`"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-card-actions>
              <v-btn
                class="error"
                @click="updateItem()"
              >
                更新
              </v-btn>
              <v-btn
                class="warning"
                @click="deleteItem()"
              >
                削除
              </v-btn>
            </v-card-actions>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </div>
</template>

<script>
/* eslint-disable no-console */
export default {
  name: 'BookEdit',
  data () {
    return {
      menu1: false,
      menu2: false,
      menu3: false,
      search: '',
      select: '',
      items: ['非', '所有'],
      book: [
        {
          ISBN13: '',
          ISBN10: '',
          BookName: '',
          Author: '',
          Publisher: '',
          Genre: '',
          IssueDate: '',
          GetDate: '',
          ReadDate: '',
          Ownership: 0,
          Purchase: 0,
          Library: '',
          Overview: '',
          Impressions: '',
          State: '',
          CoverImg: ''
        }
      ],
      inIsbn13: this.$route.query.id,
      inIsbn10: '',
      inBookName: '',
      inAuthor: '',
      inPublisher: '',
      inGenre: '',
      inIssueDate: '',
      inGetDate: '',
      inReadDate: '',
      inOwnership: 0,
      inPurchase: 0,
      inLibrary: '',
      inOverview: '',
      inImpressions: '',
      inState: '',
      inCoverImg: '',
      alertFlg: false,
      alertMessage: ''
    }
  },
  created () {
    this.searchDate()
  },
  methods: {
    async searchDate () {
      if (!this.inIsbn13) {
        window.alert('検索キーが未設定です！' + this.inIsbn13)
        return
      }
      try {
        const res = await this.$axios.$get('http://localhost:5000/search', {
          params: {
            id: this.inIsbn13
          }
        })
        this.book = res
        this.inIsbn10 = this.book[0].ISBN10
        this.inBookName = this.book[0].BookName
        this.inAuthor = this.book[0].Author
        this.inPublisher = this.book[0].Publisher
        this.inGenre = this.book[0].Genre
        if (this.book[0].IssueDate) {
          if (this.book[0].IssueDate.slice(0, 10) === '0000-00-00') {
            this.inIssueDate = ''
          } else {
            this.inIssueDate = this.book[0].IssueDate.slice(0, 10)
          }
        }
        if (this.book[0].GetDate) {
          if (this.book[0].GetDate.slice(0, 10) === '0000-00-00') {
            this.inGetDate = ''
          } else {
            this.inGetDate = this.book[0].GetDate.slice(0, 10)
          }
        }
        if (this.book[0].ReadDate) {
          if (this.book[0].ReadDate.slice(0, 10) === '0000-00-00') {
            this.inReadDate = ''
          } else {
            this.inReadDate = this.book[0].ReadDate.slice(0, 10)
          }
        }
        if (this.book[0].Ownership === 1) {
          this.select = '所有'
        } else {
          this.select = '非'
        }
        this.inOwnership = this.book[0].Ownership
        this.inPurchase = this.book[0].Purchase
        this.inLibrary = this.book[0].Library
        this.inOverview = this.book[0].Overview
        this.inImpressions = this.book[0].Impressions
        this.inState = this.book[0].State
        this.inCoverImg = this.book[0].CoverImg
      } catch (e) {
        console.log(e.errorCode) // eslint-disable-line no-console
        window.alert(e)
      }
    },
    openAmazon () {
      const url = 'https://www.amazon.co.jp/dp/' + this.inIsbn10
      window.open(url, '_blank')
    },
    async updateItem () {
      if (!this.inIsbn13) {
        window.alert('更新キーが未設定です！' + this.inIsbn13)
        return
      }
      if (!this.inBookName) {
        window.alert('書籍名は必須項目です！')
        return
      }
      if (this.select === '所有') {
        this.inOwnership = 1
      } else {
        this.inOwnership = 0
      }
      // const url = 'http://localhost:5000/update'
      try {
        await this.$axios.$post('/update',
          {
            ISBN13: this.inIsbn13,
            ISBN10: this.inIsbn10,
            BookName: this.inBookName,
            Author: this.inAuthor,
            Publisher: this.inPublisher,
            Genre: this.inGenre,
            IssueDate: this.inIssueDate,
            GetDate: this.inGetDate,
            ReadDate: this.inReadDate,
            Ownership: this.inOwnership,
            Purchase: this.inPurchase,
            Library: this.inLibrary,
            Overview: this.inOverview,
            Impressions: this.inImpressions,
            State: this.inState,
            CoverImg: this.inCoverImg
          }
        )
        window.alert('更新処理を実行しました。')
        window.close()
      } catch (e) {
        console.log(e.errorCode) // eslint-disable-line no-console
        if (e.response) {
        // err-1.要求がなされたとサーバがステータスコードで応答した
          window.alert('1 ' + e.response.data + '\n' + e.response.status + '\n' + e.response.headers)
        } else if (e.request) {
        // err-2.要求がなされたが、応答が受信されなかった
          window.alert('2 ' + e.request + '\n' + this.patchBody)
        } else {
        // err-3.トリガーしたリクエストの設定に何かしらのエラーがある
          window.alert('3 Error:' + e.errorCode + '\n' + e.message)
        }
      }
    },
    deleteItem () {
      this.deleteBook()
      window.location.href = 'http://localhost:3000/booklog'
    },
    async deleteBook () {
      if (!this.inIsbn13) {
        window.alert('削除キーが未設定です！' + this.inIsbn13)
        return
      }
      const answer = window.confirm('削除してもいいですか？')
      if (answer) {
        try {
          await this.$axios.$get('http://localhost:5000/delete', {
            params: {
              id: this.inIsbn13
            }
          })
        } catch (e) {
          console.log(e.errorCode) // eslint-disable-line no-console
          window.alert(e)
        }
      }
    },
    closeWindow () {
      const answer = window.confirm('編集中のものは保存されませんが、閉じてよろしいですか？')
      if (answer) {
        window.close()
      }
    }
  }
}
</script>
