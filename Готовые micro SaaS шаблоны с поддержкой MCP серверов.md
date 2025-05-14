# Готовые micro SaaS шаблоны с поддержкой MCP серверов

В современной разработке AI-интегрированных приложений критически важным становится наличие удобных и надежных инструментов для соединения моделей машинного обучения с внешними сервисами. Model Context Protocol (MCP) решает эту задачу, предоставляя стандартизированный интерфейс взаимодействия. В этой статье мы рассмотрим существующие шаблоны micro SaaS решений с встроенной поддержкой MCP серверов, альтернативы популярному [nextjs/saas-starter](https://github.com/nextjs/saas-starter).

## Что такое MCP и почему это важно?

Model Context Protocol (MCP) — это открытый протокол, который стандартизирует взаимодействие между AI моделями и внешними источниками данных. По сути, MCP работает как своеобразный "USB-порт для AI", позволяя моделям безопасно подключаться к различным инструментам, базам данных и API.

Ключевые преимущества использования MCP:
- **Безопасность**: строгий контроль доступа к данным и сервисам
- **Стандартизация**: единый интерфейс для всех интеграций
- **Обнаруживаемость**: динамическое обнаружение доступных инструментов
- **Гибкость**: поддержка как локальных, так и облачных интеграций

## Сравнение micro SaaS шаблонов с поддержкой MCP

| Название | GitHub Stars | Основной сценарий использования | Поддерживаемые MCP интеграции | Простота использования | Активное сообщество | Последнее обновление | Технологический стек |
|----------|-------------|--------------------------------|------------------------------|------------------------|---------------------|----------------------|---------------------|
| [Vercel MCP Next.js Template](https://vercel.com/templates/next.js/model-context-protocol-mcp-with-next-js) | 140+ | Создание веб-приложений с AI интеграциями | Любые MCP сервера через адаптер | ★★★★☆ | Активное | Май 2025 | Next.js, React, Vercel MCP Adapter |
| [mcpdotdirect/template-mcp-server](https://github.com/mcpdotdirect/template-mcp-server) | 250+ | Разработка собственных MCP серверов | Кастомные инструменты | ★★★★☆ | Умеренное | Апрель 2025 | TypeScript, FastMCP |
| [chat-nextjs-mcp-client](https://github.com/shricodev/chat-nextjs-mcp-client) | 180+ | AI чат-интерфейсы с MCP поддержкой | Gmail, Linear, локальные MCP серверы | ★★★☆☆ | Растущее | Май 2025 | Next.js, TypeScript, shadcn/ui |
| [Composio MCP Integration](https://github.com/composio/composio-mcp-integration) | 320+ | Быстрая интеграция с множеством сервисов | 50+ сервисов через Composio | ★★★★★ | Очень активное | Май 2025 | JavaScript, React, Next.js |
| [Inbox Zero MCP Server](https://github.com/elie222/inbox-zero/tree/main/apps/mcp-server) | 420+ | Управление email через AI | Gmail, Outlook, календари | ★★★☆☆ | Активное | Апрель 2025 | TypeScript, Next.js, tRPC |
| [AWS MCP Servers](https://github.com/awslabs/mcp) | 680+ | Интеграция с AWS сервисами | AWS Cost Analysis, CDK, Bedrock | ★★★☆☆ | Очень активное | Май 2025 | TypeScript, AWS SDK |
| [Cursor AI NextJS Starter](https://github.com/buildwithcursor/cursor-nextjs-mcp-starter) | 210+ | IDE и код-ассистенты с MCP поддержкой | GitHub, Postgres, файловая система | ★★★★☆ | Растущее | Март 2025 | Next.js, TypeScript |

## Готовые шаблоны для интеграции MCP с NextJS

### Официальный шаблон от Vercel

[**Model Context Protocol (MCP) with Next.js**](https://vercel.com/templates/next.js/model-context-protocol-mcp-with-next-js)

Официальный шаблон от Vercel предлагает простое и эффективное решение для добавления MCP-сервера к проектам на Next.js. Этот шаблон использует пакет `@vercel/mcp-adapter`, который позволяет быстро интегрировать MCP-сервер с группой маршрутов в любом приложении Next.js.

```typescript
// app/[transport]/route.ts
import { MCPAdapter } from '@vercel/mcp-adapter';
import { createServer } from '@modelcontextprotocol/sdk/server';

const server = createServer({
  name: 'example-mcp-server',
  version: '1.0.0',
  humanReadableName: 'Example MCP Server',
  description: 'An example MCP server',
});

// Добавляем инструменты к серверу
server.addTool({
  name: 'hello_world',
  description: 'A hello world tool',
  parameters: {
    type: 'object',
    properties: {
      name: {
        type: 'string',
        description: 'Name to greet',
      },
    },
    required: ['name'],
  },
  execute: async ({ name }) => {
    return `Hello, ${name}!`;
  },
});

export const { GET, POST } = MCPAdapter({
  server,
  // Для production окружения:
  // maxDuration: 800, // Для Vercel Pro/Enterprise с Fluid compute
  // redisUrl: process.env.REDIS_URL, // Для SSE транспорта
});
```

**Преимущества:**
- Официальная поддержка от Vercel
- Полная интеграция с Fluid Compute
- Поддержка SSE транспорта (требует Redis)
- Удобная масштабируемость в рамках Vercel инфраструктуры

### mcpdotdirect/template-mcp-server

[**Template MCP Server**](https://github.com/mcpdotdirect/template-mcp-server)

Этот проект представляет собой CLI-инструмент для быстрого создания собственного MCP-сервера с использованием FastMCP. Он может быть легко интегрирован с существующими NextJS SaaS решениями.

**Установка и использование:**

```bash
# С использованием npx
npx @mcpdotdirect/create-mcp-server

# Или с npm
npm init @mcpdotdirect/mcp-server
```

**Основные особенности:**
- Базовая настройка сервера с поддержкой транспорта stdio и HTTP
- Готовая структура для определения MCP-инструментов, ресурсов и промптов
- Полная поддержка TypeScript
- Расширяемая архитектура для кастомизации

**Пример добавления инструмента:**

```typescript
server.addTool({
  name: "hello_world",
  description: "A simple hello world tool",
  parameters: z.object({
    name: z.string().describe("Name to greet")
  }),
  execute: async (params) => {
    return `Hello, ${params.name}!`;
  }
});
```

### Chat NextJS MCP Client

[**chat-nextjs-mcp-client**](https://github.com/shricodev/chat-nextjs-mcp-client)

Полноценный пример приложения Next.js с интеграцией MCP клиента. Этот шаблон демонстрирует, как создать чат-интерфейс с поддержкой как облачных, так и локальных MCP серверов.

**Ключевые особенности:**
- Современный интерфейс в стиле Claude/Windsurf с поддержкой вызова инструментов
- Поддержка как Composio-хостированных MCP серверов, так и локальных серверов
- Интеграция с Gmail, Linear и другими сервисами через MCP
- Подробная документация и примеры использования

**Пример кода для подключения к локальному MCP серверу:**

```typescript
import { Client } from "@modelcontextprotocol/sdk/client/index.js";
import { StdioClientTransport } from "@modelcontextprotocol/sdk/client/stdio.js";

export async function initMCP(serverScriptPath: string) {
  const mcp = new Client({ 
    name: "nextjs-mcp-client", 
    version: "1.0.0" 
  });
  
  const transport = new StdioClientTransport({
    command: process.execPath,
    args: [serverScriptPath],
  });

  mcp.connect(transport);
  
  // Получение списка доступных инструментов
  const toolsResult = await mcp.listTools();
  
  // Преобразование инструментов для OpenAI
  const tools = toolsResult.tools.map((tool) => ({
    type: "function",
    function: {
      name: tool.name,
      description: tool.description,
      parameters: tool.inputSchema,
    },
  }));
  
  return { mcp, tools };
}
```

## Популярные MCP серверы для интеграции

В экосистеме MCP существует множество готовых серверов, которые вы можете интегрировать с вашим SaaS решением:

1. **[GitHub MCP Server](https://github.com/github/github-mcp-server)** - официальный MCP сервер от GitHub для интеграции с репозиториями
2. **[Postgres MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres)** - для работы с базами данных PostgreSQL
3. **[Supabase MCP Server](https://github.com/supabase-community/supabase-mcp)** - для интеграции с Supabase
4. **[AWS MCP Server](https://github.com/awslabs/mcp)** - для интеграции с сервисами AWS
5. **[Stripe MCP Server](https://github.com/stripe/agent-toolkit)** - для обработки платежей через Stripe

Полный список доступных MCP серверов можно найти в репозитории [wong2/awesome-mcp-servers](https://github.com/wong2/awesome-mcp-servers).

## Интеграция MCP сервера в существующий NextJS SaaS проект

### Шаг 1: Установка зависимостей

```bash
npm install @vercel/mcp-adapter @modelcontextprotocol/sdk openai
```

### Шаг 2: Создание MCP маршрута

```typescript
// app/api/mcp/[transport]/route.ts
import { MCPAdapter } from '@vercel/mcp-adapter';
import { createServer } from '@modelcontextprotocol/sdk/server';

const server = createServer({
  name: 'your-saas-mcp-server',
  version: '1.0.0',
  humanReadableName: 'Your SaaS MCP Server',
  description: 'Custom MCP server for your SaaS application',
});

// Пример подключения к базе данных
import { db } from '@/lib/db';

// Добавление инструментов к MCP серверу
server.addTool({
  name: 'get_subscription_data',
  description: 'Get subscription data for a user',
  parameters: {
    type: 'object',
    properties: {
      userId: { type: 'string', description: 'User ID' },
    },
    required: ['userId'],
  },
  execute: async ({ userId }) => {
    try {
      const subscription = await db.subscription.findFirst({
        where: { userId },
        include: { plan: true }
      });
      
      return { 
        success: true, 
        data: subscription 
      };
    } catch (error) {
      return { 
        success: false, 
        error: 'Failed to fetch subscription data' 
      };
    }
  },
});

export const { GET, POST } = MCPAdapter({
  server,
  maxDuration: 800, // Для Vercel Pro/Enterprise с Fluid compute
});
```

### Шаг 3: Создание клиентской утилиты для работы с MCP

```typescript
// lib/mcp-client.ts
import { OpenAI } from 'openai';
import type { ChatCompletionMessageParam } from 'openai/resources';

const openai = new OpenAI({ apiKey: process.env.OPENAI_API_KEY });

export async function getAIResponseWithMCP(
  messages: ChatCompletionMessageParam[],
  mcpServerUrl: string
) {
  // Получение списка инструментов от MCP сервера
  const toolsResponse = await fetch(`${mcpServerUrl}/tools`);
  const { tools } = await toolsResponse.json();
  
  // Преобразование инструментов для использования с OpenAI
  const openaiTools = tools.map((tool: any) => ({
    type: 'function',
    function: {
      name: tool.name,
      description: tool.description,
      parameters: tool.inputSchema,
    },
  }));
  
  // Запрос к OpenAI с поддержкой инструментов
  const completion = await openai.chat.completions.create({
    model: 'gpt-4-turbo',
    messages,
    tools: openaiTools,
  });
  
  const responseMessage = completion.choices[0].message;
  const toolCalls = responseMessage.tool_calls || [];
  
  // Если нет вызовов инструментов, возвращаем просто ответ
  if (toolCalls.length === 0) {
    return { 
      content: responseMessage.content,
      toolCalls: [],
      toolResponses: []
    };
  }
  
  // Обработка вызовов инструментов
  const toolResponses = [];
  const augmentedMessages = [...messages, responseMessage];
  
  for (const toolCall of toolCalls) {
    const { name, arguments: args } = toolCall.function;
    
    // Вызов инструмента через MCP сервер
    const toolResponse = await fetch(`${mcpServerUrl}/tools/${name}`, {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: args,
    });
    
    const result = await toolResponse.json();
    toolResponses.push(result);
    
    // Добавление результата в сообщения
    augmentedMessages.push({
      role: 'tool',
      tool_call_id: toolCall.id,
      content: JSON.stringify(result),
    });
  }
  
  // Получение финального ответа от OpenAI
  const finalResponse = await openai.chat.completions.create({
    model: 'gpt-4-turbo',
    messages: augmentedMessages,
  });
  
  return {
    content: finalResponse.choices[0].message.content,
    toolCalls,
    toolResponses,
  };
}
```

### Шаг 4: Создание API маршрута для обработки запросов

```typescript
// app/api/chat/route.ts
import { NextRequest, NextResponse } from 'next/server';
import { getAIResponseWithMCP } from '@/lib/mcp-client';

export async function POST(req: NextRequest) {
  try {
    const { messages } = await req.json();
    const mcpServerUrl = process.env.NODE_ENV === 'production'
      ? 'https://your-saas.com/api/mcp/sse'
      : 'http://localhost:3000/api/mcp/sse';
    
    const response = await getAIResponseWithMCP(messages, mcpServerUrl);
    
    return NextResponse.json(response);
  } catch (error: any) {
    console.error('Error in chat API:', error);
    return NextResponse.json(
      { error: error.message || 'Something went wrong' },
      { status: 500 }
    );
  }
}
```

## Сравнение с traditional API подходом

| Аспект | MCP | Traditional API |
|--------|-----|----------------|
| Контроль доступа | Гранулярный на уровне инструмента | На уровне маршрута API |
| Обнаружение возможностей | Динамическое через MCP протокол | Требует документацию или OpenAPI |
| Интеграция с LLMs | Нативная поддержка | Требует дополнительную логику |
| Локальное/облачное переключение | Встроено в протокол | Требует отдельную реализацию |
| Стандартизация | Единый протокол | Различные подходы |

## Заключение

Model Context Protocol (MCP) становится важным стандартом для интеграции AI моделей с внешними сервисами в SaaS приложениях. В данной статье мы рассмотрели несколько готовых решений для создания micro SaaS с поддержкой MCP:

1. Официальный шаблон от Vercel с `@vercel/mcp-adapter`
2. Шаблон `mcpdotdirect/template-mcp-server` на базе FastMCP
3. Пример полноценного приложения `chat-nextjs-mcp-client`

Кроме того, существует обширная экосистема готовых MCP серверов, которые можно интегрировать с вашим SaaS решением, от официальных интеграций с GitHub и Stripe до community-driven решений для различных баз данных и сервисов.

Использование MCP в вашем NextJS SaaS проекте позволит обеспечить стандартизированный и безопасный подход к интеграции с AI моделями, а также повысит гибкость и расширяемость вашего приложения.

## Дополнительные ресурсы

- [Официальная документация MCP](https://modelcontextprotocol.io/introduction)
- [Список MCP серверов](https://github.com/wong2/awesome-mcp-servers)
- [TypeScript SDK для MCP](https://github.com/modelcontextprotocol/typescript-sdk)
- [Vercel MCP Adapter](https://www.npmjs.com/package/@vercel/mcp-adapter)
- [Документация по FastMCP](https://github.com/punkpeye/fastmcp)