# Running Ovis2 locally without Huggingface

## Notice:

This tutorial helps to install both **AIDC-AI/Ovis** package running the model and the model itself.

The package and the model(s) should be installed in different folders.

> **:warning:** Even if running locally, it seems that running *Ovis2 server* opens 2 TCP connections toward US datacenters.
  This may lead to potential security issues.


## 1. AIDC-AI/Ovis package

Git clone AIDC-AI/Ovis package which seems to allow a user-friendly use of Ovis2

`git clone git@github.com:AIDC-AI/Ovis.git`

Follow install instructions:

https://github.com/AIDC-AI/Ovis/?tab=readme-ov-file#install

The usage of [UV from Astral](https://docs.astral.sh/uv/) is highly recommanded for the virtualenv creation and packages installation as they are heavy ones (`conda` is going to take forever).


## 2. Download the model's weights

Model's weights can be found in HuggingFace:

https://huggingface.co/AIDC-AI/Ovis2-1B/tree/main

Git clone the repository. Git-lfs should be installed to handle large files such as model's weights.
> `sudo apt install git-lfs`

Then:
```
git lfs install
git clone https://huggingface.co/AIDC-AI/Ovis2-1B
```

## 3. Running the model

Activate the virtualenv created in *1. AIDC-AI/Ovis package*.

First, check ***setuptools*** is installed in the virtualenv else pip install it:
```
(uv) pip list | grep setuptools
(uv) pip install setuptools
```

Second, check the presence of ***Nvidia CUDA Compiler***
`nvcc --version`

If no return or error, install it:
`sudo apt install nvidia-cuda-toolkit`

***Ovis*** requires *flash-attn* package. However, this python supports high performance GPUs such as H100 / H800 GPU.

More informations about GPU support by *flash-attn* here: https://github.com/Dao-AILab/flash-attention#nvidia-cuda-support

There is a workaround to bypass *flash-attn* issues.
Open the model's config file: i.e. ***Ovis2-1B/config.json*** and change (make a backup first):

`"llm_attn_implementation": "flash_attention_2",`

to:

`"llm_attn_implementation": "eager",`

You customize the behaviors of the model from the *config.json* unless you know what you are doing.

then, run:
`python ovis/serve/server.py --model_path MODEL_PATH --port PORT_NUMBER`

for instance, using UV:
`uv run ovis/serve/server.py --model_path ../Ovis2-1B --port 8888`

You can use Ovis2 locally in your browser at `127.0.0.1:PORT_NUMBER`: e.g. `127.0.0.1:8888`

