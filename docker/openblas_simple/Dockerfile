FROM python:3-slim-bookworm

# We need to set the host to 0.0.0.0 to allow outside access
ENV HOST 0.0.0.0

RUN mkdir models

# RUN wget -P /models  https://huggingface.co/TheBloke/Llama-2-7B-Chat-GGUF/resolve/main/llama-2-7b-chat.Q4_K_M.gguf
# Install the package
RUN apt update && apt install -y libopenblas-dev ninja-build build-essential pkg-config git \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/*

RUN python -m pip install --upgrade pip pytest cmake scikit-build setuptools fastapi uvicorn sse-starlette pydantic-settings starlette-context

RUN CMAKE_ARGS="-DGGML_BLAS=ON -DGGML_BLAS_VENDOR=OpenBLAS" pip install llama_cpp_python --verbose

# Run the server
CMD python3 -m llama_cpp.server --model /models/llama-2-7b-chat.Q4_K_M.gguf --host 0.0.0.0 --port 8000
