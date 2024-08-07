# MLGPU BILLING SERVER

## Install

This version is the modified python-llama-cpp server. You should build it manually.

```
git submodule update --init --recursive
make update.vendor
make build.cuda
pip install uvicorn anyio starlette fastapi sse_starlette starlette_context pydantic_settings requests
```

## Prepare

Download model into models/ folder or wherever you want.
Choose the alias for a model.

## Register

Go https://billing.mlgpu.ru, register there, got a pipenode account and open manage notebook.
Register a service, let's say 'your_llm' and register products in it as 'alias_in', 'alias_out'.
If you plan to run embedding mode, then register 'alias_emb' for embeddings.

If you use config file for models settings, you can prepare several models and sould register the products for each model.
Note, that, at present, only one model can be loaded in memory at once. Selecting another 

Finally got the *service_id* and *owner_token* from your service.

## Run

```
python3 -m llama_cpp.server --n_gpu_layers=33 --embedding=false --model models/Meta-Llama-3.1-8B-Instruct-Q6_K.gguf --model_alias "Meta-Llama-3.1-8B-Instruct-Q6_K" --owner_token "XXXX-XXXX-XXXXX-XXXX"
```
Note embedding=true for /embeddings, but this may conflict with /completion.

### Serve

Now any user, registered in https://billing.mlgpu.ru can pay you for tokens.
He should add your *service_id* to it's services list and use corresponding *user_token* for authentication.
Also he should specify a model alias in all API calls.