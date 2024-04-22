
# LangChain SubQuery Example Usage
### This document outlines how to utilize the SubQuery class and related functionality from the provided code for querying a database of tutorial videos about a software library. The code integrates various libraries and tools to decompose complex queries into actionable sub-queries.

## Classes and Methods
The core of the implementation uses the SubQuery class to define sub-queries:

from langchain_core.pydantic_v1 import BaseModel, Field

class SubQuery(BaseModel):
    """Search over a database of tutorial videos about a software library."""
    sub_query: str = Field(\n
        ...,\n
        description="A very specific query against the database.",\n
    )
Each `SubQuery` instance represents a decomposed part of a complex user query, focusing on a single aspect necessary to form a comprehensive response.

## Query Analysis and Execution

system = """You are an expert at converting user questions into database queries. \
You have access to a database of tutorial videos about a software library for building LLM-powered applications. \
Perform query decomposition. Given a user question, break it down into distinct sub questions that \
you need to answer in order to answer the original question."""

prompt = ChatPromptTemplate.from_messages([("system", system), ("human", "{question}")])
llm = ChatOpenAI(model="gpt-4", temperature=0)
llm_with_tools = llm.bind_tools([SubQuery])
parser = PydanticToolsParser(tools=[SubQuery])
query_analyzer = prompt | llm_with_tools | parser
This setup configures a query analyzer that can take a complex question, decompose it into sub-queries, and utilize the `SubQuery` instances for further processing.

## Example Usage

response = query_analyzer.invoke({"question": "how to do rag"})
print(response)
Replace "how to do rag" with any relevant question to your database.
