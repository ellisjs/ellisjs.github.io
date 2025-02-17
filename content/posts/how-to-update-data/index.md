+++
date = '2024-09-17T22:25:09+08:00'
draft = false
title = 'Next.js: 如何更新数据'
categories = ["框架"]
tags = ["JavaScript", "Next.js"]
+++

您可以使用 React 的服务器函数在 Next.js 中更新数据。本文将介绍如何创建和调用 Server Functions。

## [创建服务器](https://nextjs.org/docs/app/getting-started/updating-data#creating-server-functions)函数

可以使用 use server 来定义 Server Function 命令。您可以将该指令放在异步函数的顶部，以将该函数标记为服务器函数，或者放在单独文件的顶部，以标记该文件的所有导出。我们建议在大多数情况下使用单独的文件。

```jsx
'use server'
 
export async function createPost(formData: FormData) {}
 
export async function deletePost(formData: FormData) {}
```

### [服务器组件](https://nextjs.org/docs/app/getting-started/updating-data#server-components)

通过在函数体的顶部添加 `“use server”` 指令，可以在 Server Components 中内联服务器函数：

```jsx
export default function Page() {
  // Server Action
  async function createPost() {
    'use server'
    // Update data
    // ...
 
  return <></>
}
```

### [客户端组件](https://nextjs.org/docs/app/getting-started/updating-data#client-components)

无法在 Client Components 中定义 Server Functions。但是，您可以在 Client Components 中调用它们，方法是从顶部带有 `“use server”` 指令的文件中导入它们：

```jsx
'use server'
 
export async function createPost() {}
```

```jsx
'use client'
 
import { createPost } from '@/app/actions'
 
export function Button() {
  return <button formAction={createPost}>Create</button>
}
```

## [调用 Server 函数](https://nextjs.org/docs/app/getting-started/updating-data#invoking-server-functions)

调用服务器函数的主要方法有两种：

1. 服务器和客户端组件中的[表单](https://nextjs.org/docs/app/getting-started/updating-data#forms)
2. 客户端组件中的[事件处理程序](https://nextjs.org/docs/app/getting-started/updating-data#event-handlers)

### 表单

React 扩展了 HTML 表单元素以允许使用 HTML action 属性调用 Server Function。

在表单中调用时，该函数会自动接收 FormData 对象。您可以使用本机 FormData 方法提取数据 :

```jsx
import { createPost } from '@/app/actions'
 
export function Form() {
  return (
    <form action={createPost}>
      <input type="text" name="title" />
      <input type="text" name="content" />
      <button type="submit">Create</button>
    </form>
  )
}
```

```jsx
'use server'
 
export async function createPost(formData: FormData) {
  const title = formData.get('title')
  const content = formData.get('content')
 
  // Update data
  // Revalidate cache
}
```

### [事件处理程序](https://nextjs.org/docs/app/getting-started/updating-data#event-handlers)

您可以使用事件处理程序（如 `onClick`）在 Client Component 中调用 Server Function。

```jsx
'use client'
 
import { incrementLike } from './actions'
import { useState } from 'react'
 
export default function LikeButton({ initialLikes }: { initialLikes: number }) {
  const [likes, setLikes] = useState(initialLikes)
 
  return (
    <>
      <p>Total Likes: {likes}</p>
      <button
        onClick={async () => {
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }}
      >
        Like
      </button>
    </>
  )
}
```

### [显示待处理状态](https://nextjs.org/docs/app/getting-started/updating-data#showing-a-pending-state)

在执行服务器函数时，你可以使用 React 的 useActionState 显示加载指示器 钩。此钩子返回一个待处理的布尔值：

```jsx
'use client'
 
import { useActionState } from 'react'
import { createPost } from '@/app/actions'
import { LoadingSpinner } from '@/app/ui/loading-spinner'
 
export function Button() {
  const [state, action, pending] = useActionState(createPost, false)
 
  return (
    <button onClick={async () => action()}>
      {pending ? <LoadingSpinner /> : 'Create Post'}
    </button>
  )
}
```

### [重新验证缓存](https://nextjs.org/docs/app/getting-started/updating-data#revalidating-the-cache)

执行更新后，您可以通过在 Server Function 中调用 [revalidatePath](https://nextjs.org/docs/app/api-reference/functions/revalidatePath) 或 [revalidateTag](https://nextjs.org/docs/app/api-reference/functions/revalidateTag) 来重新验证 Next.js 缓存并显示更新的数据：

```jsx
'use server'
 
import { revalidatePath } from 'next/cache'
 
export async function createPost(formData: FormData) {
  // Update data
  // ...
 
  revalidatePath('/posts')
}
```

### [重定向](https://nextjs.org/docs/app/getting-started/updating-data#redirecting)

您可能希望在执行更新后将用户重定向到其他页面。您可以通过在 Server Function 中调用 [redirect](https://nextjs.org/docs/app/api-reference/functions/redirect) 来执行此作：

```jsx
'use server'
 
import { redirect } from 'next/navigation'
 
export async function createPost(formData: FormData) {
  // Update data
  // ...
 
  redirect('/posts')
}
```