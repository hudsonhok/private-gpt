# Running Your Own Private GPT with WSL and GPU Acceleration (NVIDIA)

[PrivateGPT](https://github.com/zylon-ai/private-gpt) is an AI project designed to enable users to upload their documents using LLMs (Large Language Models) while ensuring complete data privacy. Here's some key points:

* Offline functionality: You DON'T need an internet connection after the installation process.
* Privacy: It's designed for industries or consumers with privacy concerns including healthcare and legal to ensure their data remains fully under their control.

## Setup
So to get started with this you will need to have Windows 10 OS or higher installed along with WSL and an NVIDIA GPU.

### Install WSL
Open a PowerShell or Command Prompt in administrator mode by right-clicking and selecting "Run as administrator" and type in the following:

`wsl --install`

This will install the latest Ubuntu distribution by default.

### Update Ubuntu
Now open up a WSL terminal and type the following:
`sudo apt-get update`
`sudo apt-get upgrade`
`sudo apt-get install build-essential`

### Install NVIDIA CUDA Toolkit
Visit [Nvidia's](https://developer.nvidia.com/cuda-downloads) website to download the CUDA toolkit (12.5 as of recently)

Select Linux > x86_64 > WSL-Ubuntu > 2.0 > deb(network) and follow the installation instructions.

After installing the toolkit add the library paths to your .bashrc:

`export PATH="/usr/local/cuda-12.5/bin:$PATH"`
`export LD_LIBRARY_PATH="/usr/local/cuda-12.5/lib64:$LD_LIBRARY_PATH"`

Reload your terminal and check to make sure it's installed correctly:

`source ~/.bashrc`
`nvcc --version`
`nvidia-smi.exe`

Both commands should return some info but no errors.

### Set up Python Environment

`sudo apt-get install git gcc make openssl libssl-dev libbz2-dev libreadline-dev libsqlite3-dev zlib1g-dev libncursesw5-dev libgdbm-dev libc6-dev zlib1g-dev libsqlite3-dev tk-dev libssl-dev openssl libffi-dev lzma liblzma-dev`

`curl https://pyenv.run | bash`

`export PATH="/home/$(whoami)/.pyenv/bin:$PATH"`

Add the following to the .bashrc file (You can use Vim editor with `vim ~/.bashrc`):

`export PYENV_ROOT="$HOME/.pyenv"`

`[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"`

`eval "$(pyenv init -)"`

Reload the terminal with:

`source .bashrc`

Install Python 3.11:

`pyenv install 3.11`

`pyenv global 3.11`

`pip install pip --upgrade`

`pyenv local 3.11`

### Install Poetry

`curl -sSL https://install.python-poetry.org | python3 -`

Add the following line to your .bashrc, replacing \<USERNAME> with your WSL username (you can find it by typing `whoami` in the terminal):

`export PATH="/home/<USERNAME>/.local/bin:$PATH"`

Reload the configuration and ensure poetry is installed correctly:

`source ~/.bashrc`

`poetry --version`

### Install PrivateGPT
Clone the repository:

`git clone https://github.com/zylon-ai/private-gpt.git`

Install PrivateGPT dependencies:

`cd private-gpt`
`poetry install --extras "ui embeddings-huggingface llms-llama-cpp vector-stores-qdrant"`

### Build and Run PrivateGPT
Install LLAMA libraries with GPU Support with the following:

`CMAKE_ARGS='-DGGML_CUDA=on' poetry run pip install --force-reinstall --no-cache-dir llama-cpp-python`

This downloads an LLM locally (mistral-7b by default):

`poetry run python scripts/setup`

Run PrivateGPT

`make run`

You can verify if the GPU is being utilized by checking if blas = 1 with the run command output above.

Visit `http://127.0.0.1:8001/` on your web browser and you should see the following:

![PrivateGPT_preview](https://github.com/hudsonhok/private-gpt/assets/77293019/22ebc95a-cfef-4e66-9ca1-88c33865233d)

### Conclusion

Congratulations! You now have your own private GPT to help you on your AI journey. Enjoy!

### Credits
This was my updated working version based off of Emilien Lancelot's tutorial [here](https://dev.to/docteurrs/installing-privategpt-on-wsl-with-gpu-support-1m2a).
