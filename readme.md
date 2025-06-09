[**Go To Google Colab**](https://colab.research.google.com/drive/1wa6gnNJfApAOfByZxN8GcTZ6oQiSnsbW?usp=sharing)

-----
# Tools

The OpenAI Agents SDK provides a robust framework for integrating various tools into agents, enabling them to perform tasks such as data retrieval, web searches, and code execution. Here's an overview of the key points regarding tool integration.

### Types of tools

1. **Hosted Tools**
    - **WebSearchTool**
    - **FileSearchTool**
    - **ComputerTool**

2. **Function Calling**
3. **Agents As Tools**

## Function tools
You can use any Python function as a tool. The Agents SDK will setup the tool automatically:

- The name of the tool will be the name of the Python - function (or you can provide a name)
- Tool description will be taken from the docstring of the function (or you can provide a description)
- The schema for the function inputs is automatically created from the function's arguments
- Descriptions for each input are taken from the docstring of the function, unless disabled
- We use Python's ``inspect`` module to extract the function signature, along with `griffe` to parse docstrings and pydantic for schema creation.





## Implementing Tools :

- **Function Tools :** By decorating Python functions with @function_tool, they can be seamlessly integrated as tools for agents.

 ## Create Tool :
```python
import asyncio
from agents import Agent, Runner
from agents.tool import function_tool
from agents.run import RunConfig


@function_tool("piaic_students")
async def piaic_students(student_rollno : int) -> str:

  """
    find the student according to their roll no

  """
  students = {
       8911 : "Jin",
       8922 : "Jungkook",
       3243 : "Taehyung",
       4232 : "Jimin",
       5238 : "Suga"
   }

  return students.get(student_rollno, "Not founded")
```

## Connect Tool With Agent :

```python
async def main():
  agent = Agent(
      name = "PIAIC AGENT ",
      instructions = "you are reponse in haikus only  ",
      tools = [piaic_students],
      model = model
  )

  result = await Runner.run(agent, "share the detail of rollno-8911 ")

  print(result.final_output)


if __name__ == "__main__":
  asyncio.run(main())  
```

> output :
Roll number you seek,
The student's name is Jin,
That's all I can share.

## Agent  Visualiztion 
#### Install the optional `viz` dependency group:
`pip install "openai-agents[viz]"`


add this code on the top  `from agents.extensions.visualization import draw_graph`

pass the the agent on the `draw_graph(agent)`

#### URL : [for more detail to go Tool OpenAI SDK](https://openai.github.io/openai-agents-python/tools/)
