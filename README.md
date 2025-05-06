
# 📄 RAG Protos

This repository contains the gRPC `.proto` definitions for the RAGify microservice architecture.  
It serves as the central contract between the Node.js API gateway and the Python-based RAG engine.

---

## 🧱 Services

### `RAGProcessor`

```proto
syntax = "proto3";

package rag;

service RAGProcessor {
  rpc ParseAndEmbed (PDFRequest) returns (EmbeddingResponse);
  rpc AnswerQuery (RAGQueryRequest) returns (RAGResponse);
}

message PDFRequest {
  string filename = 1;
  bytes content = 2;
}

message EmbeddingResponse {
  string document_id = 1;
  repeated string chunk_ids = 2;
}

message RAGQueryRequest {
  string document_id = 1;
  string question = 2;
}

message RAGResponse {
  string answer = 1;
  repeated string sources = 2;
}
```

---

## 🛠️ Usage

### Generate stubs for Python

```bash
python -m grpc_tools.protoc -I. --python_out=. --grpc_python_out=. ragify.proto
```

### Generate stubs for Node.js

```bash
npx grpc_tools_node_protoc --js_out=import_style=commonjs,binary:. --grpc_out=. -I. ragify.proto
```

---

## 🔁 Versioning Strategy

- Follows **semantic versioning**: `v1.0.0`, `v1.1.0`, etc.
- Breaking changes require a **major version** bump
- Compatible changes add fields with new tags (never reuse tags)

---

## 📦 Integration Tip

You can use this as a Git submodule:

```bash
git submodule add https://github.com/your-org/ragify-protos.git proto
```

Then access `proto/ragify.proto` in both Node.js and Python services for code generation.
