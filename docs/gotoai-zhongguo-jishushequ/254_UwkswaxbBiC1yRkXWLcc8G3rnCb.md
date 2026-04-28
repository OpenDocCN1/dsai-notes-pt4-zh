# A Step-by-Step Guide to Install DeepSeek-R1 Locally with Ollama, vLLM or Transformers

> 来源：[https://gotoaiworld.feishu.cn/docx/Yv6idwNQmoREptxcBcRcJV5VnRd](https://gotoaiworld.feishu.cn/docx/Yv6idwNQmoREptxcBcRcJV5VnRd)

https://dev.to/nodeshiftcloud/a-step-by-step-guide-to-install-deepseek-r1-locally-with-ollama-vllm-or-transformers-44a1

DeepSeek-R1 is making waves in the AI community as a powerful open-source reasoning model, offering advanced capabilities that challenge industry leaders like OpenAI’s o1 without the hefty price tag. This cutting-edge model is built on a Mixture of Experts (MoE) architecture and features a whopping 671 billion parameters while efficiently activating only 37 billion during each forward pass. This approach helps balance performance and efficiency, and makes this model highly scalable and cost-effective. What sets DeepSeek-R1 apart is its unique reinforcement learning (RL) methodology, which enables it to develop chain-of-thought reasoning, self-verification, and reflection autonomously. These qualities make it an exceptional tool for tackling complex challenges across diverse fields like math, coding, and logical reasoning.

Unlike traditional LLMs, DeepSeek-R1 provides better insights into its reasoning processes and delivers optimized performance on key benchmarks.

![](img/5bb18565015bbc5c6834b784b7bf023f.png)

![](img/c840b33661cb2a9a5dd92c1f7d431633.png)

DeepSeek-R1 outperforms top models like OpenAI's o1 and Claude Sonnet 3.5 in several benchmarks.

![](img/a3e091b092d8e8ecf814237728567ae4.png)

Several methods exist out there on the internet for installing DeepSeek-R1 locally on your machine (or VM). In this guide, we will cover the three best and simplest approaches to quickly setting up and running this model on your machine. By the end of this article, you'll be able to make a thoughtful decision on which method suits your requirements the best.

Prerequisites

The minimum system requirements for running a DeepSeek-R1 model:

+   Disk Space: 500 GB (may vary across models)

+   Jupyter Notebook or Nvidia Cuda installed.

+   GPU Configuration requirements depending on the type of model are as follows:

We'll recommend you to take a screenshot of this chart and save it somewhere, so that you can quickly look up to the GPU prerequisites before trying a model.

Step-by-step process to install DeepSeek-R1 locally

For the purpose of this tutorial, we’[ll use a ](https://nodeshift.com/)GPU-powered Virtual Machine by NodeShift since it provides high compute Virtual Machines at a very affordable cost on a scale that meets GDPR, SOC2, and ISO27001 requirements. Also, it offers an intuitive and user-friendly interface, making it easier for beginners to get started with Cloud deployments. However, feel free to use any cloud provider of your choice and follow the same steps for the rest of the tutorial.

# Step 1: Setting up a NodeShift Account

Visit app.nodeshift.com and create an account by filling in basic details, or continue signing up with your Google/GitHub account.

[If yo](https://app.nodeshift.com/sign-up)u already have an account, login straight to your dashboard.

![](img/4afcb50ab72121b38a8a3f1d7cd8c1fb.png)

# Step 2: Create a GPU Node

After accessing your account, you should see a dashboard (see image), now:

1.  Navigate to the menu on the left side.

1.  Click on the **GPU Nodes** option.

1.  Click on Start to start creating your very first GPU node.

These GPU nodes are GPU-powered virtual machines by NodeShift. These nodes are highly customizable and let you control different environmental configurations for GPUs ranging from H100s to A100s, CPUs, RAM, and storage, according to your needs.

# Step 3: Selecting configuration for GPU (model, region, storage)

1.  For this tutorial, we’ll be using RTX 4090 GPU, however, you can choose any GPU of your choice as per your needs.

1.  Similarly, we’ll opt for 700GB storage by sliding the bar. You can also select the region where you want your GPU to reside from the available ones.

![](img/f507cce93fe29b7084f10ee4c7c673c8.png)

# Step 4: Choose GPU Configuration and Authentication method

1.  After selecting your required configuration options, you'll see the available VMs in your region and according to (or very close to) your configuration. In our case, we'll choose a 2x RTX 4090 GPU node with 64 vCPUs/129GB RAM/700 GB SSD.

1.  Next, you'll need to select an authentication method. Two methods are available: Password and SSH Key. We recommend using SSH keys, as they are a more secure option. To create one, head over to our official documentation.

![](img/f43f6316d416dab1c9ba6a1db046d5ba.png)

# Step 5: Choose an Image

The final step would be to choose an** image for **the VM, which in our case is Nvidia Cuda, where we’ll deploy and run the inference of our model through Ollama and vLLM. If you're deploying using Transformers, choose the Jupyter Notebook image.

That's it! You are now ready to deploy the node. Fina**lize t**he configuration summary, and if it looks good, click Create to deploy the node.

![](img/ec6cf547a2aa9af1f393629b5ee5405c.png)

![](img/ecace67491316c85f23e4b0ac38c9bb5.png)

# Step 6: Connect to active Compute Node using SSH

1.  As soon as you create the node, it will be deployed in a fe**w secon**ds or a minute. Once deployed, you will see a status Running in green, meaning that our Compute node is ready to use!

1.  Once your GPU shows this status, navigate to the three dots on the right, click on Connect with SSH, and copy the SSH details that appear.

![](img/db9f0f2c7d25f50690d277b7bd5bd1eb.png)

As you copy the details, follow the below steps to connect to the running GPU VM via SSH:

1.  Open your terminal, paste the SSH command, and run it.

1.  In some cases, your terminal may take your consent before connecting. Enter ‘yes’.

1.  A prompt will request a password. Type the SSH password, and you should be connected.

Output:

![](img/dc526cfe46a0125c623ca2d3f1a04744.png)

Installation using Ollama

Ollama is a user-friendly option for quickly running DeepSeek-R1 locally with minimal configuration. It's best suited for individuals or small-scale projects that don't require extensive optimization or scaling.

Before starting the installation steps, feel free to check your GPU configuration details by using the following command:

```
nvidia-smi
```

Output:

![](img/840efb964be645eb3b600861d55e829d.png)

The first method of installation will be through Ollama. For installing DeepSeek-R1 with Ollama, follow the steps given below:

1.  Install Ollama

```
curl -fsSL https://ollama.com/install.sh | sh
```

Output:

As the installation completes, you may get the following warning:

```
WARNING: Unable to detect NVIDIA/AMD GPU. Install lspci or lshw to automatically detect and install GPU dependencies.
```

This may happen when Ollama is not able to detect the GPU in your system automatically. To resolve this, simply install some GPU dependencies with the following command:

```
sudo apt install pciutils lshw
```

After this, rerun the Ollama installation command, and this time, it should successfully detect and use the GPU.

1.  Confirm installation by checking the version.

```
ollama --version
```

Output:

![](img/ed142aa1d974561f4966f222e2f1f51f.png)

1.  Start Ollama.

Once the installation is done, we'll start the Ollama server in the current terminal and do the rest of the operations in a new terminal.

```
ollama serve
```

Output:

Now that our Ollama server has been started, let's install the model.

1.  Open a new terminal window and run the ollama command to check if everything is up and running and to see a list of Ollama commands.

Output:

![](img/a982426132764788ca182963ca955487.png)

1.  Run the DeepSeek-R1 model with the following command.

(replace <MODEL_CODE> with your preferred model type, e.g., 70b)

```
ollama run deepseek-r1:&lt;MODEL_CODE&gt;
```

Output:

![](img/c2a871d96faccb10f008d9474dee0980.png)

The model will take some time to finish downloading; once it's done, we can move forward with model inference.

1.  Give prompts for model inference.

Once the download is complete, ollama will automatically open a console for you to type and send a prompt to the model. This is where you can chat with the model. For e.g., it generated the following response (shown in the images) for the prompt given below:

"Explain the difference between monorepos and turborepos"

Output:

![](img/e0402b82895f04f5e8f80b06f82bc601.png)

![](img/5b265ba17a57a8f10e0f97b4944c2125.png)

Installation using vLLM

vLLM is designed for efficient inference with optimized memory usage and high throughput, which makes it ideal for production environments. Choose this if you need to serve large-scale applications with performance and cost efficiency in mind.

In the upcoming steps, you'll see how to install DeepSeek-R1 using vLLM.

Make sure you have a new server to perform this setup. If you've already installed the model using Ollama, you can either skip this method or install it on a new server to prevent memory shortage.

1.  Confirm if Python is installed.

```
python3 -V
```

Output:

![](img/2381e33d9bb40765ce5da0b4fdbe10d6.png)

1.  Install pip.

```
apt install -y python3-pip
```

Output:

![](img/a21ab8196a6a300808fdee5390474366.png)

1.  Install Rust and Cargo packages as dependencies for vLLM using rustup.

```
curl --proto &#x27;=https&#x27; --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

Output:

![](img/30eb9c2821f115b4775359aad901d700.png)

1.  Confirm installation

```
rustc --version
cargo --version
```

Output:

![](img/2a3c2a6ebadcb092859ec38594897d41.png)

1.  Install vLLM

```
pip install vllm
```

Output:

![](img/ddd49c15690cddbaf34030082f7d13d2.png)

As shown in the above image, you may encounter an error in the middle of the installation process because of an incompatible version of transformers. To fix this, run the following command:

```
pip install transformers -U
```

Output:

After fixing the error, run the vllm installation command again, and it should be downloaded without any errors.

1.  Load and run the model.

For the scope of this tutorial, we'll run the DeepSeek-R1-Distill-Llama-8B model with vLLM. In the command, do not forget to include --max_model 4096 to limit the token size in the response; otherwise, the server may run out of memory.

```
vllm serve &quot;deepseek-ai/DeepSeek-R1-Distill-Llama-8B&quot; --max_model 4096
```

Output:

![](img/5074ff28c7fb769ddaf1e472ddd90aeb.png)

1.  Open a new terminal and call the model server using the following command.

Replace the "content" attribute with your prompt. For e.g., our prompt is "Tell me the recipe for tea".

```
curl -X POST &quot;http://localhost:8000/v1/chat/completions&quot; \
    -H &quot;Content-Type: application/json&quot; \
    --data &#x27;{
        &quot;model&quot;: &quot;deepseek-ai/DeepSeek-R1-Distill-Llama-8B&quot;,
        &quot;messages&quot;: [
            {
                &quot;role&quot;: &quot;user&quot;,
                &quot;content&quot;: &quot;Tell me the recipe for tea&quot;
            }
        ]
    }&#x27;
```

Output:

![](img/1b33e531dc58b85aca3d2731dbc7664b.png)

### Installation using Transformers

Transformers offers maximum flexibility and control for fine-tuning and experimenting with DeepSeek-R1. It's the best choice for developers and researchers who need to customize models for their specific use cases and experiment with various training or inference configurations.

In this section, you will learn to install the model using Transformers. We'll install and run the model with Python code on Jupyter Notebook.

1.  To** use the built-**in Jupyter Notebook** functionality **on your remote server, follow the same steps (Step 1 — Step 6) to create a new GPU instance, but this time, select the Jupyter Notebook option instead of Nvidia Cuda in the Choose an Image section and deploy the GPU.

![](img/41ba0ca6f3f2e07f826c4209c3c5768f.png)

1.  After the GPU is running, click Connect with SSH to open a Jupyter Notebook session on your browser.

![](img/5e1c2a4704786ba44da94cfbf5c80d1d.png)

1.  Open a Python Notebook.

1.  Install dependencies to run the model with Transformers.

```
!pip install transformers accelerate
```

Output:

1.  Load and run the model using a pipeline from Transformers.

For demonstration of this method, we are running the DeepSeek-R1-Distill-Qwen-1.5B model. You can replace it with your preferred one as per the requirements.

```
# Use a pipeline as a high-level helper
from transformers import pipeline

messages = [
    {&quot;role&quot;: &quot;user&quot;, &quot;content&quot;: &quot;How can you help me?&quot;},
]
pipe = pipeline(&quot;text-generation&quot;, model=&quot;deepseek-ai/DeepSeek-R1-Distill-Qwen-1.5B&quot;)
pipe(messages)
```

Output:

### Conclusion

In this guide, we've explored three different methods to install DeepSeek-R1 locally—Ollama, vLLM, and Transformers, each offering unique benefits depending on your requirements, whether it's ease of use, performance optimization, or flexibility. By understanding these approaches, you can efficiently deploy DeepSeek-R1 in a way that best suits your workflow. With NodeShift Cloud, managing such deployments becomes even more streamlined, providing a robust infrastructure that simplifies setup, and enhances scalability, ensuring a seamless experience for developers looking to harness the power of DeepSeek-R1 with minimal operational overhead.

**For more informat**ion about NodeShift:

+   [Website](https://nodeshift.com/?ref=blog.nodeshift.com)

+   [Docs](https://docs.nodeshift.com/?ref=blog.nodeshift.com)

+   [LinkedIn](https://www.linkedin.com/company/nodeshift/?%0Aref=blog.nodeshift.com)

+   [X](https://x.com/nodeshiftai?ref=blog.nodeshift.com)

+   [Discord](https://discord.gg/4dHNxnW7p7?ref=blog.nodeshift.com)

+   [daily.dev](https://app.daily.dev/nodeshift?ref=blog.nodeshift.com)