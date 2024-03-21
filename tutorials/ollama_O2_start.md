## Starting Ollama as a singularity container on O2

### overview
This will try to demonstrate how to get ollama starting on a gpu node on O2. Currently HMSRC has built a singularity image in a shared directory meaning ollama can be setup in a compute node to leverage O2 HPC resources for serving LLM models. This tutorial will walk you through the steps needed to run this singulairty container. Note that once set up, it can only recieve API calls from within O2.


1. allocate resources in a interactive node, we will use the `gpu_ccb` partition for this demo but you can run this on the regular `gpu` queue.

Request a node from SLUM
`srun --account=gentleman_rcg7_contrib --pty -p interactive -t 0-01:00 --mem 16G -c 4 -p gpu_ccb --gres=gpu:1 /bin/bash`

Once request is fufilled and in the node, load modules
`module load gcc/9.2.0 cuda/12.1 python/3.10.11`

2. start a tmux session in that interactive session (also take note of the name of the node you're working on)

`tmux new -s ollamasession`

3. run the ollama container, be sure to put the model weights on the scratch folder

Create enviromental variables to put models in scratch. This is where the model weights will be stored so for best performance it should be placed on the fastest drive storage.
` export APPTAINERENV_OLLAMA_MODELS="<SCRATCH DIR>/ollama/models`

Start singularity apptainer and mounting scratch storage to apptainer
` singularity instance start --nv -B <SCRATCH DIR> /n/app/singularity/containers/shared/ollama-0.1.27.sif "ollama"`

Open shell in apptainer
`singularity shell instance://ollama`

Start the ollama system daemon
`Apptainer> ollama serve`

Exit the tmux session and return to the node.
`ctrl + B , d`


4. after following all those steps you can now use ollama to have a CLI for models on http://127.0.0.1:11434
curl -X POST http://127.0.0.1:11434/api/generate -d '{
  "model": "llama2",
  "prompt":"Write a haiku about the blue sky"
 }'