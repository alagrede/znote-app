# Local LLM Assistant


```bash
npm i -S @langchain/community@0.0.33
npm i -S @langchain/core@^0.1.40
npm i -S hnswlib-node@^1.4.2
npm i -S langchain@^0.0.103
```


```js blockName=local-assistant-api;
import { DirectoryLoader } from "langchain/document_loaders/fs/directory";
import { TextLoader } from "langchain/document_loaders/fs/text";
import { JSONLoader } from "langchain/document_loaders/fs/json";
import { MemoryVectorStore } from "langchain/vectorstores/memory";
import { ChatOllama } from "@langchain/community/chat_models/ollama";
import { OllamaEmbeddings } from "@langchain/community/embeddings/ollama";
import { HNSWLib } from "langchain/vectorstores/hnswlib";
import { RecursiveCharacterTextSplitter } from "langchain/text_splitter";
import { RunnableSequence } from "@langchain/core/runnables";
import { StringOutputParser } from "@langchain/core/output_parsers";
import { PromptTemplate } from "@langchain/core/prompts";

const VECTOR_STORE_PATH = "/Documents.index";
const MODEL_NAME = "llama2"; //or mistral


export default class LocalAssistantAPI {


  constructor (basePath, stdout) {
    this.stdout = stdout;
    this.basePath = basePath;
    // Load local files such as .json and .txt from ./docs
    this.loader = new DirectoryLoader(this.basePath + "/docs", {
      ".json": (path) => new JSONLoader(path),
      ".txt": (path) => new TextLoader(path)
    })
  }
  

  // Define a function to normalize the content of the documents
  normalizeDocuments(docs) {
    return docs.map((doc) => {
      if (typeof doc.pageContent === "string") {
        return doc.pageContent;
      } else if (Array.isArray(doc.pageContent)) {
        return doc.pageContent.join("\n");
      }
    });
  }

  async reindex() {
    console.log("Loading docs...")
    const docs = await this.loader.load();

    console.log('Creating new vector store...')
    const textSplitter = new RecursiveCharacterTextSplitter({
      chunkSize: 1000,
    });
    const normalizedDocs = this.normalizeDocuments(docs);
    const splitDocs = await textSplitter.createDocuments(normalizedDocs);

    const embeddings = new OllamaEmbeddings({
      baseUrl: "http://localhost:11434",
      model: MODEL_NAME,
      maxConcurrency: 5,
    });

    // Load docs and save into DB
    const vectorStore = await HNSWLib.fromDocuments(
        splitDocs,
        embeddings
    );
    await vectorStore.save(this.basePath + VECTOR_STORE_PATH);
    console.log("Embeddings save!");
  }

  async completion(question) {

    const model = new ChatOllama({
      baseUrl: "http://localhost:11434/",
      model: MODEL_NAME,
      options: {
        temperature: 0.5
      }
    });

    const embeddings = new OllamaEmbeddings({
      baseUrl: "http://localhost:11434",
      model: MODEL_NAME,
      maxConcurrency: 5,
    });

    // Load the vector store from the same directory
    const vectorStore = await HNSWLib.load(this.basePath + VECTOR_STORE_PATH, embeddings);


    const questionPrompt = PromptTemplate.fromTemplate(
        `In addition to your knowledge, you can optionally use the following pieces of context to answer the question at the end.
----------
CONTEXT: {context}
----------
QUESTION: {question}
----------
Helpful Answer:`
    );


    const formatDocumentsAsString = (documents) => documents.map((doc) => doc.pageContent).join("\n\n");

    const retriever = vectorStore.asRetriever();

    const chain = RunnableSequence.from([
      {
        question: (input) => input.question,
        context: async (input) => {
          const relevantDocs = await retriever.getRelevantDocuments(input.question);
          const serialized = formatDocumentsAsString(relevantDocs);
          return serialized;
          //return '';
        },
      },
      questionPrompt,
      model,
      new StringOutputParser(),
    ]);


    console.log("Querying chain...")

    const stream = await chain.stream({
      question: question,
    });

    for await (const chunk of stream) {
      this.stdout(chunk);
    }

  }
}
```


##  👋  Write Assistant API Here
**Write assistant API above into a JS script**
```js
//exec: node
const fs = require("node:fs");
const script = loadBlock('local-assistant-api');
fs.writeFileSync(`${__dirname}/local-assistant-api.js`, script);
print("DONE!");
```

# 🧑‍💻 Use the Assistant API
```js
//exec: node
//external: hnswlib-node
import LocalAssistantAPI from "./local-assistant-api.js";
const assistant = new LocalAssistantAPI(__dirname, printInline);
//assistant.reindex();
assistant.completion("Who is Google?");
```

