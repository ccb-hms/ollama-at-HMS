# ollama-at-HMS :llama:
Documentation, scripts, tutorials, presentations, wikis, etc. that relate to running and interacting with text LLM models using `ollama` on HMS O2


# FAQ


## What is going on?

Right now, there is a running job on our dedicated GPU node that runs [Ollama](https://github.com/ollama/ollama) - a popular and open-source service that runs and serves LLM models locally. We can therefore use this node as an endpoint for LLM applications:

```bash
Request access by email <ccbhelp@hms.harvard.edu>
```
This is **only accessible via the HMS network** so you will need to connect directly to HMS internet or be on the VPN

If  installed locally, the ollama endpoint by default will be on localhost address. 
```bash
http://127.0.0.1:11434
```

The endpoint has a RESTful API that can be queried using any language, including Python, JavaScript, Typescript, Go, Rust, and even R. Learn more about using the API in the **[API Documentation](./api.md)**.

For further details on how the backend of Ollama works, see this [blogpost](https://eli.thegreenplace.net/2024/the-life-of-an-ollama-prompt/).

## Can we run (some LLM Model) on O2?

Ollama supports a list of models available on [ollama.com/library](https://ollama.com/library 'ollama model library'). See the API docs for how to make model pull requests.

Create new models or modify models already in the library using the Modelfile. See the **[Modelfile Documentation](./modelfile.md)** or the instructions on **[import](./import.md)**.

## Is there a UI I can use to chat with the models?

There is already a large collection of plugins available for VSCode as well as other editors that leverage Ollama. See the list of [extensions & plugins](https://github.com/jmorganca/ollama#extensions--plugins) at the bottom of the main repository readme.

For off-the-shelf UI options, you can use the [OpenWebUI](https://github.com/open-webui/open-webui) project which has a ready-built docker container that will interface with our ollama endpoint. If you can run docker on your local machine, simply run the following command:
  ```bash
  docker run -d -p 3000:8080 -e OLLAMA_BASE_URL=<endpoint URL>:11434 -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
  ```

After the container builds and runs, you can access Open WebUI at [http://localhost:3000](http://localhost:3000). Sign up using any name and fake email and it will make you an admin.

## How do I use this in my application

Many libraries now integrate with ollama with wrappers that give it the same api as OpenAI functions. See the **[tutorials](Tutorials.md)** for examples.

## Do all my requests run on the GPU?
At the moment the server is using 1 A100GPU and all requests are completed in serial. Multiple requests will be completed in the order recieved.


#### Citations
<sub> most of these docs are shamelessly ripped off from the [ollama docs](https://github.com/ollama/ollama/tree/main/docs)</sub>