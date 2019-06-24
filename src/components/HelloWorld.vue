<template>
  <cube-form
    class="task-other"
    :model="model"
    :options="options"
    @validate="validateHandler">
    <cube-form-group>
      <cube-form-item :field="fields[0]"></cube-form-item>
      <cube-form-item :field="fields[1]"></cube-form-item>
      <cube-form-item :field="fields[2]">
        <cube-button @click="showDatePicker">{{model.dateValue || '请选择截止日期'}}</cube-button>
      </cube-form-item>
      <cube-form-item :field="fields[3]"></cube-form-item>
      <cube-form-item :field="fields[4]"></cube-form-item>
      <cube-form-item :field="fields[5]"></cube-form-item>
      <cube-form-item :field="fields[6]"></cube-form-item>
      <cube-form-item :field="fields[7]"></cube-form-item>
      <cube-form-item :field="fields[8]"></cube-form-item>
      <div class="qa">
        <p class="title">帮助:</p>
        <ul>
          <li><a href="javascript:void(0);" class="a-blue">视频教程</a></li>
          <li><a href="javascript:void(0);" class="a-blue">商家手续费说明</a></li>
          <li><a href="javascript:void(0);" class="a-blue">发布任务教程</a></li>
          <li><a href="javascript:void(0);" class="a-blue">开发者api接口</a></li>
        </ul>
      </div>
    </cube-form-group>
  </cube-form>
</template>

<script>
  // const taskTypes = ['注册', '投票', '关注', '浏览', '转发', '发帖', '评价', '高价', '特单', '长单', '电商', '抖音']
  export default {
    name: 'task-other',
    data () {
      return {
        validity: {},
        valid: undefined,
        model: {
          qrCodeType: 0,
          title: '',
          dateValue: '',
          remark: '',
          taskPrice: '',
          taskCount: '',
          taskLimit: '',
          taskAmount: '0.00',
          uploadValue: [],
          validateWords: 'asdaaaaa',
          checkboxGroupValue: [],
          radioValue: '',
          rateValue: 0,
          selectValue: 2018,
          switchValue: true,
          textareaValue: 'sssss'
        },
        satrDesc: [
          '所有用户可接',
          '完成10条任务且成功率大于70%',
          '完成20条任务且成功率大于75%',
          '完成40条任务且成功率大于75%',
          '完成80条任务且成功率大于75%',
          '完成160条任务且成功率大于75%',
          '完成320条任务且成功率大于75%'
        ],
        fields: [
          // 支持设备
          {
            type: 'radio-group',
            modelKey: 'qrCodeType',
            label: '支持设备',
            props: {
              horizontal: true,
              max: 1,
              options: [
                {
                  label: '苹果',
                  value: 0
                },
                {
                  label: '安卓',
                  value: 1
                },
                {
                  label: '全部',
                  value: 2
                }
              ]
            },
            rules: {
              required: true
            },
            messages: {
              required: '请选择一个类型'
            }
          },
          // 标题
          {
            type: 'input',
            modelKey: 'title',
            label: '标题',
            props: {
              placeholder: '请输入项目名称+核心要求'
            },
            rules: {
              required: true
            },
            // validating when blur
            trigger: 'blur'
          },
          // 截止时间
          {
            modelKey: 'dateValue',
            label: '截止时间',
            rules: {
              required: true
            }
          },
          // 赏金
          {
            type: 'input',
            modelKey: 'taskPrice',
            label: '赏金',
            props: {
              type: 'number',
              // 这个需要根据任务类型切换
              placeholder: '最低价格0.5元起'
            },
            rules: {
              required: true,
              pattern: /(^([1-9]\d*)(\.\d{1,2})?$)|(^[0]\.[1-9]\d?$)|(^[0]\.[0][1-9]$)/
            },
            messages: {
              pattern: '整数或者保留2位小数'
            },
            // validating when blur
            trigger: 'blur'
          },
          // 数量
          {
            type: 'input',
            modelKey: 'taskCount',
            label: '数量',
            props: {
              type: 'number',
              // 这个需要根据任务类型切换
              placeholder: '最低5条起'
            },
            rules: {
              required: true,
              pattern: /^([1-9]\d*)$/
            },
            messages: {
              pattern: '只能输入整数'
            },
            // validating when blur
            trigger: 'blur'
          },
          // 总价
          {
            type: 'input',
            modelKey: 'taskAmount',
            label: '总价',
            props: {
              type: 'number',
              readonly: true
              // 这个需要根据任务类型切换
            },
            rules: {
              required: true
            },
            messages: {
            },
            // validating when blur
            trigger: 'blur'
          },
          // 用户可领取次数
          {
            type: 'input',
            modelKey: 'taskLimit',
            label: '单用户领取次数',
            props: {
              type: 'number',
              // 这个需要根据任务类型切换
              placeholder: '请输入每个用户可领取次数'
            },
            rules: {
              required: true,
              pattern: /^([1-9]\d*)$/
            },
            messages: {
              pattern: '只能输入整数'
            },
            // validating when blur
            trigger: 'blur'
          },
          // 操作说明
          {
            type: 'upload',
            modelKey: 'qrCodeImg',
            label: '操作说明',
            props: {
              max: 3
            },
            rules: {
              required: true
            },
            // validating when blur
            trigger: 'blur'
          },
          // 文字验证
          {
            type: 'textarea',
            modelKey: 'validateWords',
            label: 'Textarea',
            rules: {
              required: true
            },
            // debounce validate
            // if set to true, the default debounce time will be 200(ms)
            debounce: 100
          }
        ],
        options: {
          scrollToInvalidField: true,
          // classic fresh
          layout: 'standard' // standard classic fresh
        }
      }
    },
    methods: {
      showDes () {
        alert('展示星级说明')
      },
      validateHandler (result) {
        this.validity = result.validity
        this.valid = result.valid
        console.log('validity', result.validity, result.valid, result.dirty, result.firstInvalidFieldIndex)
      },
      showDatePicker () {
        if (!this.dateTimePicker) {
          this.dateTimePicker = this.$createDatePicker({
            title: '截止时间',
            min: new Date(),
            max: new Date(2020, 9, 20, 20, 59, 59),
            value: new Date(),
            columnCount: 6,
            onSelect: this.selectHandle,
            onCancel: this.cancelHandle
          })
        }
        this.dateTimePicker.show()
      },
      selectHandle (date, selectedVal, selectedText) {
        console.log(typeof date)
        console.log(selectedVal)
        console.log(selectedText)
        this.model.dateValue = `${selectedText[0]}-${selectedText[1]}-${selectedText[2]} ${selectedText[3]}:${selectedText[4]}:${selectedText[5]}`
      },
      cancelHandle () {
        this.$createToast({
          type: 'correct',
          txt: 'Picker canceled',
          time: 1000
        }).show()
      }
    }
  }
</script>
<style lang="scss">
  .task-other{
    .cube-form-item{
      .cube-btn{
        padding-left: 0;
        padding-right: 0;
        text-align: left;
        /*color: inherit;*/
        background: none;
        border: none;
        color: #666666;
        font-size: 14px;
      }
    }
  }
</style>
<style lang="scss" scoped>
  .price-advice{
    padding: 0 15px;
    .title{
      padding: 5px 0;
    }
    .info {
      p{
        display: inline-block;
        color: #ccc;
        &:last-of-type{
          margin-left: 5px;
        }
      }
    }
  }
  .rate-des{
    padding: 0 15px;
  }
  .qa{
    margin-top: 15px;
    border-top: 5px solid #ccc;
    padding: 0 15px 10px 15px;
  }
</style>
