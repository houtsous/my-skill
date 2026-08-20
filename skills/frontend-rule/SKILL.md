---
name: frontend-rule
description: 前端开发规范技能。用于 React、Vue、TypeScript、Electron 渲染进程、H5、后台管理系统等前端项目的编写、修改、重构与审查，覆盖目录边界、命名、类型、接口、状态管理、路由、样式、枚举常量和魔法值治理。
---

# 前端开发规范

## 目录与边界

- 页面放 `pages/` 或 `views/`，复用组件放 `components/`，路由放 `router/` 或 `routes/`，状态放 `store/` 或 `stores/`，接口放 `api/`，非 HTTP 服务放 `services/`，工具函数放 `utils/`，常量和枚举放 `constants/`、`enum/` 或 `enums/`，类型放 `types/` 或 `api/types/`。
- 新文件放入职责明确的现有目录；只有现有目录无法表达新职责时才新建目录。
- 禁止创建含义模糊的 `common/`、`misc/`、`temp/`、`helpers/` 等目录或文件。
- `index.ts` 只作为目录入口或统一导出文件，不堆积具体业务实现。
- 不要手工修改或存放源码到 `node_modules/`、`dist/`、`out/` 等依赖或构建产物目录。

## 命名规范

- 目录名称使用小写短横线命名。
- 普通文件使用小驼峰命名；React 组件文件和 class 类文件使用大驼峰命名。
- TypeScript 类型、接口和 namespace 使用大驼峰命名；普通变量和函数使用小驼峰命名。
- 固定值、枚举对象和枚举成员使用全大写下划线命名。
- 业务接口、服务、状态管理、枚举等普通代码文件保留项目后缀规范，例如 `userProfile.api.ts`、`smsCode.service.ts`、`routeMeta.enum.ts`。
- 类型导入使用 `import type`，除非该符号运行时确实需要。

## API 约束

- 新增 API 时，先判断字段值是否需要抽成枚举或常量；只有其他页面、组件、工具函数、状态管理也可能复用的值才放到 `enum/`、`enums/` 或 `constants/`。
- 只服务于单个 API 域的请求、响应和领域类型放到 `api/types/<domain>` 中，不放全局类型目录。
- API 类型按 namespace 组织，领域名使用 `I<Domain>` 风格，内部固定区分 `Request` 和 `Response`。
- API 函数只负责 URL、请求方法、参数组装、返回类型和必要请求配置，不重复实现全局请求拦截、业务状态码判断和通用错误包装。
- 组件和状态管理模块不直接调用 Axios；通过 API 层或服务层调用。

以 `queryUserInfo` 为例：

1. 判断是否需要枚举。`role` 后续可能用于页面展示、权限判断或其他业务逻辑，应抽成可复用枚举。

```ts
export const LOGIN_ROLE = {
  ADMIN: 1,
  MEMBER: 2
} as const

export const LOGIN_ROLE_NAME = {
  [LOGIN_ROLE.ADMIN]: '管理员',
  [LOGIN_ROLE.MEMBER]: '普通成员'
} as const

export type LOGIN_ROLE_VALUE = (typeof LOGIN_ROLE)[keyof typeof LOGIN_ROLE]
```

2. 在 `api/types/user` 中定义 API 域类型。`UserInfo` 如果只服务于用户 API，就放在 `IUser` 中；如果确认会被多个页面、工具或状态模块复用，再考虑沉淀到更公共的领域类型或枚举目录。

```ts
import type { LOGIN_ROLE_VALUE } from '@renderer/enum/user.enum'

export namespace IUser {
  export type UserInfo = {
    userId: string
    name: string
    email: string
    phone: number
    role: LOGIN_ROLE_VALUE
  }

  export namespace Request {
    export type QueryUser = {
      userId: string
    }
  }

  export namespace Response {
    export type UserData = ApiResponse.Response<UserInfo>
  }
}
```

3. 在 `src/api/user` 中创建请求函数，参数类型从 `IUser.Request` 读取。

```ts
export function queryUserInfo(data: IUser.Request.QueryUser) {
  return simpleRequestClient.post<IUser.UserInfo>('/user/info', data, {
    // 其他配置如 baseURL
  })
}
```

## 枚举与魔法值

- 禁止在业务代码中散落无语义的数字、字符串、状态码、路由名、事件名、存储 key、权限 key、业务类型值。
- 遇到 `returnCode === 1`、`status === 'success'`、`type === 2`、`route.name === 'home'` 这类判断时，先抽成枚举或常量，再在业务代码中引用。
- 枚举对象使用 `as const` 保留字面量类型，并导出由枚举派生的联合类型。

## 路由

- 每个业务路由必须有稳定的 `name`；业务判断优先使用 `name`，不要依赖容易变化的 `path`。
- 路由 `name`、菜单 key、权限 key、面包屑 key、埋点 pageName 应从统一枚举或常量中读取。
- 动态参数只作为跳转参数或查询条件传递，不替代稳定路由标识。

## Electron

- Electron 项目必须区分 `main`、`preload`、`renderer` 三个运行边界。
- 渲染进程不得直接导入 Node 模块或 `src/main/`。
- 系统能力必须通过主进程、预加载脚本和 IPC 暴露最小接口。

## 样式与界面

- 优先使用项目已有 UI 框架、主题标记、UnoCSS/Tailwind/SCSS 方案，不另起一套视觉系统。
- 共享主题变量统一维护在项目现有主题目录；不要同时维护多个同义目录，例如 `theme` 与 `themes` 并存。
- 组件样式尽量靠近组件；全局样式只放基础重置样式、全局布局、主题变量和公共工具。
- 不要提交依赖生产环境的 `console.log`。
- 移动端项目需检查 rem、viewport、低版本 WebView 和宿主能力降级。
