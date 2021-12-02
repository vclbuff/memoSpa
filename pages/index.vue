<template>
  <div>
    <h1>
      Hello Memo
    </h1>
    <div>
      <label for="title">タイトル</label>
      <input v-model="title" type="text" name="title" />
      <label for="memo">メモ</label>
      <input v-model="memo" type="text" name="memo" />
      <button @click="postData">送信</button>
    </div>
    <table>
      <tr>
        <th>id</th>
        <th>title</th>
        <th>memo</th>
      </tr>
      <tr
        v-for="memo in memos"
        :key="memo.id"
      >
        <td><span @click="deleteData(memo.id)">{{ memo.id }}</span></td>
        <td>{{ memo.title }}</td>
        <td>{{ memo.memo }}</td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data () {
    return {
      memos: [],
      title: '',
      memo: ''
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      const url = 'http://localhost:3000/memos'
      this.$axios.get(url)
        .then((response) => {
          this.memos = response.data
        })
    },
    postData () {
      const url = 'http://localhost:3000/memos'
      const params = {
        title: this.title,
        memo: this.memo
      }
      this.$axios.post(url, params)
        .then(() => {
          this.getData()
          this.title = ''
          this.memo = ''
        })
    },
    deleteData (id) {
      const url = 'http://localhost:3000/memos'
      const target = `${url}/${id}`
      this.$axios.delete(target)
        .then(() => {
          this.getData()
        })
    }
  }
}
</script>
