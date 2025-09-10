---
hip: 16
title: Document Processing Standard
author: Hanzo AI Team
type: Standards Track
category: Interface
status: Draft
created: 2025-01-09
requires: HIP-4
---

# HIP-16: Document Processing Standard

## Abstract

This proposal defines the document processing standard for financial and business documents. All document processing MUST use this interface.

**Repository**: [github.com/hanzoai/az2](https://github.com/hanzoai/az2)  
**Port**: 8000 (API), 5173 (UI)

## Motivation

We need ONE standard way to:
- Process financial documents
- Extract structured data
- Generate reports

## Specification

### Document Input

```typescript
interface Document {
  id: string;
  type: "pdf" | "image" | "excel" | "csv";
  content: string;  // base64 or URL
  metadata?: Record<string, any>;
}
```

### Processing Result

```typescript
interface ProcessingResult {
  documentId: string;
  extractedData: Record<string, any>;
  confidence: number;
  tables?: Table[];
  summary?: string;
}
```

### API Endpoints

```yaml
POST /api/documents/process
  Body: Document
  Response: ProcessingResult
  
GET /api/documents/:id/status
  Response: {status: "processing" | "completed" | "failed"}
```

## Implementation

Document processing uses LLM Gateway (HIP-4) for AI extraction:

```
Document → Az2 (HIP-16) → LLM Gateway (HIP-4) → Structured Data
```

## References

1. [HIP-4: LLM Gateway](./hip-4.md)

## Copyright

Copyright and related rights waived via [CC0](https://creativecommons.org/publicdomain/zero/1.0/).