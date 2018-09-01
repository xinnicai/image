<template>
<div>
  <card>
    <div class="search-con search-con-top">
      <Input v-model="value1" placeholder="病人ID" class="search-input" />
      <Input @on-change="handleClear" clearable placeholder="姓名" class="search-input" v-model="searchValue"/>
      <DatePicker type="date" placeholder="结束日期" class="search-input"></DatePicker>
      <DatePicker type="date" placeholder="开始日期" class="search-input"></DatePicker>
      <Button @click="handleSearch" class="search-btn" type="primary"><Icon type="search"/>&nbsp;&nbsp;搜索</Button>
    </div>
    <Table :data="tableData1" :columns="tableColumns1" stripe></Table>
    <div style="margin: 10px;overflow: hidden">
        <div style="float: right;">
            <Page :total="100" :current="1" @on-change="changePage"></Page>
        </div>
    </div>
  </card>
  </div>
</template>

<script>
import './index.less'
export default {

  data () {
    return {
      searchValue: '',
      tableData1: this.mockTableData1(),
      tableColumns1: [
        {
          title: '检查ID',
          key: 'studyId'
        },
        {
          title: '病人ID',
          key: 'patientId'
        },
        {
          title: '姓名',
          key: 'patientName'
        },
        {
          title: '性别',
          key: 'sex'
        },
        {
          title: '出生日期',
          key: 'birthday'
        },
        {
          title: '检查时间',
          key: 'checktime'
        },
        {
          title: '医院',
          key: 'organ_code'
        },
        {
          title: '分析结果',
          key: 'tag'
        }
      ]
    }
  },
  methods: {
    mockTableData1 () {
      let data = []
      for (let i = 0; i < 10; i++) {
        data.push({
          name: 'Business' + Math.floor(Math.random() * 100 + 1),
          status: Math.floor(Math.random() * 3 + 1),
          portrayal: ['City', 'People', 'Cost', 'Life', 'Entertainment'],
          people: [
            {
              n: 'People' + Math.floor(Math.random() * 100 + 1),
              c: Math.floor(Math.random() * 1000000 + 100000)
            },
            {
              n: 'People' + Math.floor(Math.random() * 100 + 1),
              c: Math.floor(Math.random() * 1000000 + 100000)
            },
            {
              n: 'People' + Math.floor(Math.random() * 100 + 1),
              c: Math.floor(Math.random() * 1000000 + 100000)
            }
          ],
          time: Math.floor(Math.random() * 7 + 1),
          update: new Date()
        })
      }
      return data
    },
    formatDate (date) {
      const y = date.getFullYear()
      let m = date.getMonth() + 1
      m = m < 10 ? '0' + m : m
      let d = date.getDate()
      d = d < 10 ? ('0' + d) : d
      return y + '-' + m + '-' + d
    },
    changePage () {
      // The simulated data is changed directly here, and the actual usage scenario should fetch the data from the server
      this.tableData1 = this.mockTableData1()
    },
    handleClear (e) {
      if (e.target.value === '') this.insideTableData = this.value
    },
    handleSearch () {
      this.insideTableData = this.value.filter(item => item[this.searchKey].indexOf(this.searchValue) > -1)
    }
  }
}
</script>
