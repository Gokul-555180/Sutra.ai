# SUTRA.ai — Internal API & Provider Interface Design

> **Document Version:** 1.0.0  
> **Status:** Foundational Architecture  
> **Related Documents:** [AI_SYSTEM_DESIGN.md](AI_SYSTEM_DESIGN.md), [ARCHITECTURE.md](../../ARCHITECTURE.md), [SECURITY_DESIGN.md](SECURITY_DESIGN.md)

---

## 1. Scope & Design Boundaries

This document defines the **internal software contracts, TypeScript interfaces, and IPC boundaries** for SUTRA.ai.

> [!NOTE]
> SUTRA.ai is a desktop application communicating directly with external AI services via official HTTPS endpoints. This document specifies our **internal abstraction contracts**, not third-party proprietary endpoints. For vendor-specific HTTP schemas, consult the official documentation for [Google Gemini](https://ai.google.dev/), [Groq](https://console.groq.com/docs), [OpenAI](https://platform.openai.com/docs), [Anthropic](https://docs.anthropic.com/), and [Ollama](https://github.com/ollama/ollama/blob/main/docs/api.md).

---

## 2. Core Provider Abstraction (`IAIProvider`)

```typescript
import { z } from 'zod';

/**
 * Model capabilities metadata describing context limits and feature flags.
 */
export interface ModelMetadata {
  id: string;
  name: string;
  providerId: 'gemini' | 'groq' | 'openai' | 'anthropic' | 'ollama' | 'mock';
  contextWindow: number;
  maxOutputTokens: number;
  supportsStreaming: boolean;
  supportsJsonSchema: boolean;
  supportsVision: boolean;
  isLocal: boolean;
}

/**
 * Normalized input payload accepted by all AI providers.
 */
export interface NormalizedRequest {
  model: string;
  systemInstruction?: string;
  messages: Array<{
    role: 'user' | 'assistant' | 'system';
    content: string;
  }>;
  temperature?: number;
  maxTokens?: number;
  topP?: number;
  stopSequences?: string[];
  responseFormat?: 'text' | 'json';
}

/**
 * Streaming token chunk emitted during real-time generation.
 */
export interface StreamChunk {
  delta: string;
  finishReason?: 'stop' | 'length' | 'content_filter' | null;
}

/**
 * Standardized completion response returned across all providers.
 */
export interface NormalizedResponse {
  content: string;
  rawUsage: {
    promptTokens: number;
    completionTokens: number;
    totalTokens: number;
  };
  finishReason: 'stop' | 'length' | 'content_filter' | 'error';
  latencyMs: number;
}

/**
 * Common contract for all AI inference adapters.
 */
export interface IAIProvider {
  readonly id: string;
  readonly name: string;
  readonly supportedModels: ModelMetadata[];

  initialize(credentials?: string, endpoint?: string): Promise<void>;
  complete(request: NormalizedRequest): Promise<NormalizedResponse>;
  stream(request: NormalizedRequest): AsyncIterable<StreamChunk>;
  countTokens(text: string, model: string): number;
  validateCredentials(): Promise<boolean>;
}
```

---

## 3. Standardized Error Handling Structure

To prevent disparate vendor error responses from polluting the UI, all adapters catch HTTP or SDK exceptions and throw a structured `ProviderError`:

```typescript
export type ProviderErrorCode = 
  | 'RATE_LIMIT_EXCEEDED'
  | 'INVALID_CREDENTIALS'
  | 'CONTEXT_LENGTH_EXCEEDED'
  | 'SERVICE_UNAVAILABLE'
  | 'NETWORK_TIMEOUT'
  | 'CONTENT_POLICY_VIOLATION'
  | 'UNKNOWN_ERROR';

export class ProviderError extends Error {
  constructor(
    public readonly code: ProviderErrorCode,
    public readonly providerId: string,
    public readonly originalMessage: string,
    public readonly retryAfterMs?: number,
    public readonly actionableAdvice?: string
  ) {
    super(`[${providerId}] ${code}: ${originalMessage}`);
    this.name = 'ProviderError';
  }
}
```

---

## 4. Native Desktop IPC Command Contracts (Tauri 2)

All communication between the React frontend and Rust native layer passes through strongly typed Tauri IPC commands:

| IPC Command | Direction | Parameters | Return Type | Description |
| :--- | :--- | :--- | :--- | :--- |
| `save_credential` | Frontend → Rust | `{ providerId: string, apiKey: string }` | `Result<void, string>` | Stores key in OS Keyring. |
| `get_credential` | Frontend → Rust | `{ providerId: string }` | `Result<Option<string>, string>` | Retrieves key on demand. |
| `delete_credential`| Frontend → Rust | `{ providerId: string }` | `Result<void, string>` | Removes key from OS Keyring. |
| `db_execute` | Frontend → Rust | `{ sql: string, params: unknown[] }` | `Result<QueryResult, string>` | Executes SQL write inside transaction. |
| `db_query` | Frontend → Rust | `{ sql: string, params: unknown[] }` | `Result<unknown[], string>` | Executes read-only SQL query. |
| `read_context_file`| Frontend → Rust | `{ filePath: string }` | `Result<string, string>` | Safely reads local UTF-8 context file. |
| `backup_database` | Frontend → Rust | `{ destinationPath: string }` | `Result<void, string>` | Executes SQLite vacuum backup. |
