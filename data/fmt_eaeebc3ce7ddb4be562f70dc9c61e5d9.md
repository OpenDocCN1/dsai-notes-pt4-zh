
```jsx
<div className="bg-purple-50 p-3 rounded-lg shadow mb-4 text-[10px] text-gray-700">

  <h2 className="text-sm font-bold text-purple-600 mb-2 pb-1 border-b border-purple-200 flex items-center">

    <Briefcase size={14} className="mr-1" />

    个人简介

  </h2>

  <p>{resumeData.summary}</p>

</div>

<div className="bg-purple-50 p-3 rounded-lg shadow mb-4">

  <h2 className="text-sm font-bold text-purple-600 mb-2 pb-1 border-b border-purple-200 flex items-center">

    <GraduationCap size={14} className="mr-1" />

    教育背景

  </h2>

  {resumeData.education.map((edu, index) => (

    <div key={index} className="mb-2">

      <h3 className="text-xs font-bold text-purple-600">{edu.degree} - {edu.school}</h3>

      <p className="text-[10px] text-gray-500">{edu.period}</p>

    </div>

  ))}

</div>

<div className="bg-purple-50 p-3 rounded-lg shadow flex-1">

  <h2 className="text-sm font-bold text-purple-600 mb-2 pb-1 border-b border-purple-200 flex items-center">

    <Briefcase size={14} className="mr-1" />

    工作经历

  </h2>

  {resumeData.experience.map((job, index) => (

    <div key={index} className="mb-3">

      <h3 className="text-xs font-bold text-purple-600">{job.title} - {job.company}</h3>

      <p className="text-[10px] text-gray-500 mb-1">{job.period}</p>

      <ul className="list-none pl-0 m-0">

        {job.responsibilities.map((resp, idx) => (

          <li key={idx} className="text-[10px] text-gray-700 mb-0.5 pl-2 relative">

            <span className="absolute left-0 top-1.5 w-1 h-1 bg-indigo-500 rounded-full"></span>

            {resp}

          </li>

        ))}

      </ul>

    </div>

  ))}

</div>

</div>

</div>

);

};

export default A4Resume;
```

## 技术实现
- `React`框架（`import React from 'react'`; `import { Star, Mail, Phone, MessageCircle, Award, Briefcase, GraduationCap } from 'lucide-react'`;）
- Tailwind CSS（样式管理）
- Lucide React（图标库）
- 注意：使用A4纸竖版固定尺寸：宽595px × 高842px，任何情况下都保持简历的A4纸张比例，其他元素相应缩放。
- 配色：根据用户的职业从下面配色方案中选择合适的颜色定制简历 [`"Lavender Fields"`, `"Autumn Warmth"`, `"Ocean Twilight"`, `"Forest Dew"`, `"Coral Reef"`, `"Aurora Borealis"`, `"Desert Sunset"`, `"Mint Fresh"`, `"Berry Sweet"`, `"Urban Gray"`]。默认使用`"Lavender Fields"`配色，使用柔和的配色方案和设计感的布局，配色有设计感。
- 代码中 `""` 号内的内容使用单引号！

## 工作流：
从步骤 1 开始工作。在接收用户提供的信息后，严格按照要求直接输出最终结果，不需要额外说明

## 怎么使用？

1. 确保你能够使用 Claude Artifacts
1. 复制提示词到 Claude
1. 输入你的简历或者个人介绍，岗位JD等信息

## 为什么使用代码？

提示词中不嵌入代码，要实现这么多细节的把控，长度也会很长。使用效果也还可以，但是不够，很多细节也不可能精准把控：

![](img/b21ca06186c11c8e80d403438141f0b5.png)

使用代码，效果更加强大：

![](img/f05b7fabc24696e292a0f8f2730ec5ae.png)

## 已知问题

1. Claude Artifacts 中竖版显示有问题？本人也不太懂代码，Artifacts 中貌似有一些限制，因此是横版。
1. 代码运行不成功？重试即可。多次不成功，新开对话。
1. 想更换颜色？生成后给 Claude 说明：`“更换一套配色方案”` 即可。
1. 达到Claude 最大输出限制？重试，或者继续生成。
1. 提示词中不用代码可以吗？可以，美观度就会差很多。
1. 怎么生成竖版提示词？本地搭建 `react` 环境，复制代码本地运行即为 A4 竖版。
1. 后续是否有网站？前端代码没搞定，欢迎全栈大佬一起来搞！欢迎金主爸爸赞助！

## 更新和开源

项目持续更新，开源地址，欢迎 Star 和贡献

`https://github.com/langgptai/AI-resume`

记得点赞收藏转发本文hh！
