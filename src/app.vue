<template>
  <div class="examples">
    <div>
      <t-button @click="button1">执行setHighlight</t-button>
      <t-button @click="button5">执行unsetHighlight</t-button>
      <t-button @click="button2">执行insertContent</t-button>
      <t-button @click="button3">执行appendContent</t-button>
      <t-button @click="button4">执行replaceContent</t-button>
      <t-button @click="button5">执行unsetHighlight</t-button>
      <t-button @click="button6">重新坐标</t-button>
    </div>

    <div class="box">
      <umo-editor ref="editorRef" v-bind="options" />
    </div>
    <!-- <div class="box">
      <umo-editor editor-key="testaaa" :toolbar="{ defaultMode: 'classic' }" />
    </div> -->
  </div>
</template>

<script setup lang="ts">
import { shortId } from '@/utils/short-id'

const editorRef = $ref(null)
const templates = [
  {
    title: '工作任务',
    description: '工作任务模板',
    content:
      '<h1>工作任务</h1><h3>任务名称：</h3><p>[任务的简短描述]</p><h3>负责人：</h3><p>[执行任务的个人姓名]</p><h3>截止日期：</h3><p>[任务需要完成的日期]</p><h3>任务详情：</h3><ol><li>[任务步骤1]</li><li>[任务步骤2]</li><li>[任务步骤3]...</li></ol><h3>目标：</h3><p>[任务需要达成的具体目标或结果]</p><h3>备注：</h3><p>[任何额外信息或注意事项]</p>',
  },
  {
    title: '工作周报',
    description: '工作周报模板',
    content:
      '<h1>工作周报</h1><h2>本周工作总结</h2><hr /><h3>已完成工作：</h3><ul><li>[任务1名称]：[简要描述任务内容及完成情况]</li><li>[任务2名称]：[简要描述任务内容及完成情况]</li><li>...</li></ul><h3>进行中工作：</h3><ul><li>[任务1名称]：[简要描述任务当前进度和下一步计划]</li><li>[任务2名称]：[简要描述任务当前进度和下一步计划]</li><li>...</li></ul><h3>问题与挑战：</h3><ul><li>[问题1]：[描述遇到的问题及当前解决方案或需要的支持]</li><li>[问题2]：[描述遇到的问题及当前解决方案或需要的支持]</li><li>...</li></ul><hr /><h2>下周工作计划</h2><h3>计划开展工作：</h3><ul><li>[任务1名称]：[简要描述下周计划开始的任务内容]</li><li>[任务2名称]：[简要描述下周计划开始的任务内容]</li><li>...</li></ul><h3>需要支持与资源：</h3><ul><li>[资源1]：[描述需要的资源或支持]</li><li>[资源2]：[描述需要的资源或支持]</li><li>...</li></ul>',
  },
]
const options = $ref({
  toolbar: {
    // defaultMode: 'classic',
    // menus: ['base'],
    importWord: {
      async onCustomImportMethod() {
        return await Promise.resolve({
          value: '<p>测试导入word</p>',
        })
      },
    },
  },
  document: {
    title: '测试文档',
    content: localStorage.getItem('document.content') ?? '<p>测试文档</p>',
    characterLimit: 10000,
  },
  page: {
    showBookmark: true,
  },
  templates,
  cdnUrl: 'https://cdn.umodoc.com',
  shareUrl: 'https://umodoc.com',
  file: {
    // allowedMimeTypes: [
    //   'application/pdf',
    //   'image/svg+xml',
    //   'video/mp4',
    //   'audio/*',
    // ],
  },
  ai: {
    assistant: {
      enabled: true,
      async onMessage() {
        return await Promise.resolve('<p>AI助手测试</p>')
      },
    },
  },
  user: {
    id: 'umoeditor',
    label: 'Umo Editor',
    avatar: 'https://tdesign.gtimg.com/site/avatar.jpg',
  },
  users: [
    { id: 'umodoc', label: 'Umo Team' },
    { id: 'Cassielxd', label: 'Cassielxd' },
    { id: 'Goldziher', label: "Na'aman Hirschfeld" },
    { id: 'SerRashin', label: 'SerRashin' },
    { id: 'ChenErik', label: 'ChenErik' },
    { id: 'china-wangxu', label: 'china-wangxu' },
    { id: 'Sherman Xu', label: 'xuzhenjun130' },
    { id: 'testuser', label: '测试用户' },
  ],
  async onSave(content: string, page: number, document: { content: string }) {
    localStorage.setItem('document.content', document.content)
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        const success = true
        if (success) {
          console.log('onSave', { content, page, document })
          resolve('操作成功')
        } else {
          reject(new Error('操作失败'))
        }
      }, 2000)
    })
  },
  async onFileUpload(file: File & { url?: string }) {
    if (!file) {
      throw new Error('没有找到要上传的文件')
    }
    console.log('onUpload', file)
    await new Promise((resolve) => setTimeout(resolve, 3000))
    return {
      id: shortId(),
      url: file.url ?? URL.createObjectURL(file),
      name: file.name,
      type: file.type,
      size: file.size,
    }
  },
  onFileDelete(id: string, url: string) {
    console.log(id, url)
  },
})

const button1 = () => {
  editorRef.setHighlight({ from: 54, to: 56 }, 'red')
}
const button2 = () => {
  editorRef.insertContent('新中国')
}
const button3 = () => {
  editorRef.appendContent('新中国')
}
const button4 = () => {
  editorRef.replaceContent({ from: 54, to: 56 }, '环境1')
}
const button5 = () => {
  editorRef.unsetHighlight({ from: 5, to: 10 })
}

const data = $ref([
  {
    sentence: '第五届联合国环境大会第二阶段会议在肯尼亚首都蒙巴萨举行。',
    senIdx: 0,
    errorType: 10,
    errorTypeInfo: '归属错误:首都归属',
    errorWord: '肯尼亚首都蒙巴萨',
    startPos: 17,
    endPos: 25,
    senStartPos: 17,
    senEndPos: 25,
    suggestType: 0,
    suggestions: [
      {
        weight: 1,
        collateWord: '肯尼亚首都内罗毕',
      },
    ],
    suggestion: null,
    collateWord: '肯尼亚首都内罗毕',
    reference: null,
    weight: 1.0,
    engine: 'col',
    fileName: null,
  },
  {
    sentence:
      '2022年3月3，联合国环境规划署(ULEP)在总部所在地内罗毕举行了纪念成立50周年特别会议，环境部长黄润秋以视频方式出席会议并发言。',
    senIdx: 2,
    errorType: 16,
    errorTypeInfo: '缩略词错误',
    errorWord: 'ULEP',
    startPos: 88,
    endPos: 92,
    senStartPos: 18,
    senEndPos: 22,
    suggestType: 0,
    suggestions: [
      {
        weight: 1,
        collateWord: 'United Nations Environment Programme',
      },
      {
        weight: 1,
        collateWord: 'UNEP',
      },
    ],
    suggestion: null,
    collateWord: 'United Nations Environment Programme,UNEP',
    reference: null,
    weight: 1.0,
    engine: 'col',
    fileName: null,
  },
  {
    sentence:
      '2022年3月3，联合国环境规划署(ULEP)在总部所在地内罗毕举行了纪念成立50周年特别会议，环境部长黄润秋以视频方式出席会议并发言。',
    senIdx: 2,
    errorType: 6,
    errorTypeInfo: '领导人称谓错误',
    errorWord: '环境部长黄润秋',
    startPos: 118,
    endPos: 125,
    senStartPos: 48,
    senEndPos: 55,
    suggestType: 0,
    suggestions: [
      {
        weight: 1,
        collateWord: '第十四届全国政协常委黄润秋',
      },
      {
        weight: 1,
        collateWord: '九三学社第十五届中央副主席黄润秋',
      },
      {
        weight: 1,
        collateWord: '生态环境部部长黄润秋',
      },
    ],
    suggestion: null,
    collateWord:
      '第十四届全国政协常委黄润秋,九三学社第十五届中央副主席黄润秋,生态环境部部长黄润秋',
    reference: null,
    weight: 1.0,
    engine: 'col',
    fileName: null,
  },
  {
    sentence:
      '2022年3月3，联合国环境规划署(ULEP)在总部所在地内罗毕举行了纪念成立50周年特别会议，环境部长黄润秋以视频方式出席会议并发言。',
    senIdx: 0,
    errorType: 4,
    errorTypeInfo: '日期格式错误',
    errorWord: '2022年3月3',
    startPos: 70,
    endPos: 78,
    senStartPos: 0,
    senEndPos: 8,
    suggestType: 1,
    suggestions: null,
    suggestion: null,
    collateWord: '日期格式错误(成分缺失错误)',
    reference: null,
    weight: 1.0,
    engine: 'date',
    fileName: null,
  },
])

const button6 = () => {
  const a = {
    errorWord: '幻境',
    startPos: 54,
    endPos: 56,
    collateWord: '环境1',
  }
  const b = {
    errorWord: '幻境',
    startPos: 54,
    endPos: a.startPos + a.collateWord.length,
    collateWord: '环境1',
  }
  console.log(data)
  const offset = b.endPos - a.endPos
  for (let i = 0; i < data.length; i++) {
    if (data[i].startPos > b.startPos) {
      data[i].startPos += offset
      data[i].endPos += offset
    }
  }
  console.log(data)
}
</script>

<style>
html,
body {
  padding: 0;
  margin: 0;
}
.examples {
  margin: 20px;
  display: flex;
  height: calc(100vh - 40px);
}
.box {
  border: solid 1px #ddd;
  box-sizing: border-box;
  position: relative;
  width: 100%;
  height: 100%;
}

html,
body {
  height: 100vh;
  overflow: hidden;
}
</style>
