<template>
  <div class="container">
    <div>
      <h1 class="titled">
        {{ title }}
      </h1>
      <vue-mermaid
        :nodes="data"
        :config="mermaid"
        type="graph LR"
        @nodeClick="editNode"
      ></vue-mermaid>
      <div class="inputt">
        Set Parent :
        <input v-model="tgtparents" class="compwht" />
      </div>
      <div class="inputt">
        Set Text : <input v-model="newText" class="compwht" />
        <button :disabled="isButtonAble" class="compwht" @click="addNewNode">
          Add
        </button>
      </div>
      <br />
      check parent exist :{{ isexistparent }}
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      tgtparents: '',
      newText: '',
      currentmaxid: 6,
      title: 'vue-mermaid demo',
      mermaid: {
        theme: 'default',
        startOnLoad: !1,
        securityLevel: 'loose'
      },
      data: [
        {
          id: '1',
          text: 'A',
          link: '---',
          next: ['2'],
          editable: true,
          style: 'fill:#f9f,stroke:#333,stroke-width:4px'
        },
        { id: '2', text: 'B', edgeType: 'circle', next: ['3'] },
        { id: '3', text: 'C', next: ['4', '6'], editable: true },
        { id: '4', text: 'D', link: '-- This is the text ---', next: ['5'] },
        { id: '5', text: 'E', editable: true },
        { id: '6', text: 'F' }
      ]
    }
  },
  computed: {
    isexistparent(event) {
      return !this.isButtonAble ? 'OK' : 'NG'
    },
    isButtonAble(event) {
      return this.filterByParents(this.tgtparents) === null
    },
    nowNextA(event) {
      return this.data[0].next
    }
  },
  methods: {
    editNode(nodeId) {
      const data = this.filterById(nodeId)
      alert('clicked node = ' + data.text)
    },
    filterByParents(text) {
      const dataarr = this.data
      for (let i = 0; i < this.currentmaxid; i++) {
        if (dataarr[i].text === text) return dataarr[i]
      }
      return null
    },
    filterById(id) {
      const dataarr = this.data
      for (let i = 0; i < this.currentmaxid; i++) {
        if (dataarr[i].id === id) return dataarr[i]
      }
      return null
    },
    addNewNode(event) {
      const parent = this.filterByParents(this.tgtparents)
      const tagetId = parent.id
      const newtext = this.newText === '' ? 'new_el' : this.newText
      this.currentmaxid++
      for (let i = 0; i < this.currentmaxid - 1; i++) {
        if (this.data[i].id === tagetId) {
          const newel = String(this.currentmaxid)
          if (this.data[i].next === undefined) {
            this.data[i].next = []
          }
          this.data[i].next.push(newel)
        }
      }
      const newNode = {
        id: this.currentmaxid,
        text: newtext
      }
      this.data.push(newNode)
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.titled {
  margin: auto;
  align-items: center;
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 50px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}

.inputt {
  margin-top: 10px;
}

.compwht {
  background-color: #c7c7c7;
  margin-top: 5px;
}

.v-application--wrap {
  background-color: #fff;
  /* background-color: #979797; */
}
</style>
