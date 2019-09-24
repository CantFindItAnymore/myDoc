### 一. axios封装（附loading，防抖）

```javascript
/*
  axios.js
*/

import React from 'react'
import ReactDOM from 'react-dom'
import axios from 'axios'
import { baseURL, timeout } from './config'
import Loading from '../components/Loading'
import { Modal } from 'antd'

export default class Axios {
  // axios封装
  static ajax(option) {
    console.log(option)
    if(!option) {
      return
    }

    // 防抖
    let timer

    // axios参数对象
    let conf = {
      url: baseURL + option.url,
      headers: {
        'Content-type': "application/json"
      },
      methods: option.methods || 'GET',
      timeout
    }

    // 判断post/get以确定参数
    if (!option.methods || option.methods === 'GET') {
      conf.params = option.data || '' // get参数
    } else if (option.methods === 'POST') {
      conf.data = option.data || {} // post参数
    } else {
      return
    }

    // 根据参数来判断是否显示loading层
    let loading;
    if (option && option.showLoading === true) {
      loading = document.getElementById('loading')
      ReactDOM.render(<Loading />, document.getElementById('loading'))
      loading.style.display = 'block'
    }

    // 返回promise
    return new Promise((resolve, reject) => {
      clearTimeout(timer)
      timer = setTimeout(() => {
        axios(conf)
          .then(res => {
            let data = res.data
            if (res.status === 200) {
              if (data.code === 0) {
                resolve(data)
              } else {
                Modal.info({
                  title: '提示',
                  content: 'IO成功但请求失败'
                })
                reject(data.msg)
              }
            } else {
              reject('返回code不为0')
            }
          })
          .catch(() => {
            reject('io失败')
          })
          .finally(() => {
            // 最后都清除loading层
            if (option && option.showLoading === true) {
              loading = document.getElementById('loading')
              loading.style.display = 'none'
            }
          })
      }, 100)
    })
  }
}
```



### 二. antd表单组件封装

由于antd的form组件用起来相对复杂，所以我们应该将antd的form组件封装成**`BaseForm`**组件，使用时接收一个对象数据并渲染出表单。这样就不用每次都去调用antd的form组件了。

#### 1. BaseForm

```javascript
/*
  BaseForm组件
*/

import React, { Component } from 'react'
import { Input, Select, Form, Button, Checkbox } from 'antd'
import { getOptionList } from '../../utils/utils'

const FormItem = Form.Item


class BaseForm extends Component {
  initFormList () {
    const  { getFieldDecorator } = this.props.form
    const formList = this.props.formList
    console.log(formList)
    const formItemList = []

    // 生成各类表单元素
    if (formList && formList.length > 0) {
      // eslint-disable-next-line array-callback-return
      formList.map(item => {
        let type = item.type
        let field = item.field
        let label = item.label
        let initialValue = item.initialValue
        let placeholder = item.placeholder
        let list = item.list
        let width = item.width
        switch (type) {
          case 'SELECT':
            const SELECT = <FormItem label={label} key={field}>
              {
                getFieldDecorator((field), {
                  initialValue: initialValue
                })(
                  <Select
                    style={{width: width}}
                    placeholder={placeholder}
                  >
                    {getOptionList(list)}
                  </Select>
                )
              }
            </FormItem>
            formItemList.push(SELECT)
            console.log('select')
            break;
          case 'INPUT':
            const INPUT = (
              <FormItem label={label} key={field}>
                {getFieldDecorator}({field}, {
                  initialValue
                })(
                  <Input
                    type='text'
                    style={{width}}
                    placeholder={placeholder}
                  />
                )
              </FormItem>
            )
            formItemList.push(INPUT)
            console.log('input')
            break;
          case 'CHECKBOX':
            const CHECKBOX = (
              <FormItem label={label} key={field}>
                {getFieldDecorator}([field], {{
                  valuePropName: 'checked',
                  initialValue // true||false
                }})(
                  <Checkbox
                    style={{width}}
                    placeholder={placeholder}
                  >
                    {label}
                  </Checkbox>
                )
              </FormItem>
            )
            formItemList.push(CHECKBOX)
            console.log('checkbox')
            break;
          default:
            break;
        }
      })
      return formItemList
    }
  }

  handleOK () {
    let formValue = this.props.form.getFieldsValue()
    // 交给父组件处理
    this.props.submit(formValue)
  }

  handlereset () {
    this.props.form.resetFields()
  }

  render() {
    return (
      <Form>
        { this.initFormList() }
        <FormItem>
          <Button onClick={this.handleOK.bind(this)}>确定</Button>
          <Button onClick={this.handlereset.bind(this)}>重置</Button>
        </FormItem>
      </Form>
    )
  }
}


export default BaseForm = Form.create({})(BaseForm)


```



#### 2. utils中的getOptionList

```javascript
/*
  utils
*/

import React from 'react'
import { Select } from 'antd'
const Option = Select.Option


const getOptionList = (data) => {
  // 生成Select组件的options
  if (!data) {
    return []
  }
  let options = []
  // eslint-disable-next-line array-callback-return
  data.map(item => {
    options.push(<Option value={item.name} key={item.id}>{item.name}</Option>)
  })
  return options
}

export {
  getOptionList
}
```



#### 3. 使用时

```javascript
/*
  FormPackage
*/

import React, { Component } from 'react'
import BaseForm from '../../components/BaseForm'

export default class FormPackage extends Component {
  // BaseForm要用到的渲染数据数组(结构化)
  formList = [
    {
        type:'SELECT',
        label:'城市',
        field:'city',
        placeholder:'全部',
        initialValue:'1',
        width:80,
        list: [{ id: '0', name: '全部' }, { id: '1', name: '北京' }, { id: '2', name: '天津' }, { id: '3', name: '上海' }]
    },
    {
        type: '时间查询'
    },
    {
        type: 'SELECT',
        label: '订单状态',
        field:'order_status',
        placeholder: '全部',
        initialValue: '1',
        width: 80,
        list: [{ id: '0', name: '全部' }, { id: '1', name: '进行中' }, { id: '2', name: '结束行程' }]
    }
  ]

  submit (params) {
    console.log(params)
  }

  render() {
    return (
      <div>
        <BaseForm formList={this.formList} submit={this.submit} />
      </div>
    )
  }
}

```



