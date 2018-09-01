<template>
  <div>
    <Card>
      <tables ref="tables" searchable search-place="top" v-model="tableData" :columns="columns" :url="queryUrl" @on-delete="handleDelete"/>
    </Card>
    <Modal
        v-model="modal1"
        title="检查序列"
        @on-cancel="cancle">
        <div>
          <Table ref="listtables" :data="listTableData" :columns="listColumns" ></Table>
        </div>
    </Modal>
  </div>
</template>

<script>
import Tables from '_c/tables'
import axios from 'axios'
export default {
  name: 'tables_page',
  components: {
    Tables
  },
  data () {
    return {
      // 请求地址
      queryUrl: 'query',
      // 弹出框
      modal1: false,
      // 弹出框表格行列名称
      listColumns: [
        {
          title: '检查序列',
          key: 'seriesNumber',
          render: (h, params) => {
            let url = params.row.urlValue + '&studyid=' + params.row.studyId + '&organ_code=' + params.row.organ_code
            return h('a', {
              attrs: {
                href: url,
                target: '_black'
              }
            }, params.row.seriesNumber)
          }
        },
        {
          title: '图像数量',
          key: 'count'
        }
      ],
      // 弹出框表格数据
      listTableData: [
        {
          seriesNumber: 2,
          count: 1
        },
        {
          seriesNumber: 1,
          count: 1
        }
      ],
      // 表格行列名称
      columns: [
        {
          title: '检查ID',
          key: 'studyId',
          width: 300,
          render: (h, params) => {
            // let urlValue = sessionStorage.getItem('imageViewUrlPrefix')
            return h('a', {
              attrs: {
                href: params.row.urlValue,
                target: '_black'
              }
            }, params.row.studyId)
          }
        },
        {
          title: '病人ID',
          key: 'patientId',
          width: 300
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
          key: 'tag',
          render: (h, params) => {
            const row = params.row
            const color = row.tag === '异常' ? 'error' : 'success'
            const text = row.tag === '异常' ? '异常' : '正常'

            return h('Tag', {
              props: {
                type: 'dot',
                color: color
              }
            }, text)
          }
        },
        {
          title: '操作',
          key: 'active',
          align: 'center',
          render: (h, params) => {
            let urlValue = params.row.urlValue
            let id = params.row.studyId
            let code = params.row.organ_code
            let handle = []
            let see = h('Button', {
              props: {
                // type: 'ghost',
                // size: 'small',
                icon: 'md-eye'
              },
              style: {
                marginRight: '5px'
              },
              on: {
                click: () => {
                  this.getListData(urlValue, id, code)
                  this.modal1 = true
                }
              }
            }, '查看')

            handle.push(see)

            return h('div', handle)
          }

        }
      ],
      tableData: []
    }
  },
  methods: {
    handleDelete (params) {
      console.log(params)
    },
    cancle () {
      this.modal1 = false
    },
    getListData (urlValue, id, code) {
      axios({
        method: 'get',
        url: this._URL_INTERFACE + 'querystudy?organ_code=' + code + '&studyid=' + id

      }).then((response) => {
        debugger
        this.listTableData = response.data.data // 返回数据
      }).catch((error) => {
        console.log(error)
      })
    }

  },
  mounted () {
    // this.getTableData()
  }
}
</script>
