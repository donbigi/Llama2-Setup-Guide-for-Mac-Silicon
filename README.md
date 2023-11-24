# Llama2 Installation Guide for Mac (M1 Chip)

Guide for setting up and running Llama2 on Mac systems with  Apple silicon. This repo provides instructions 
for installing prerequisites like Python and Git, cloning the necessary repositories, downloading and converting 
the Llama models, and finally running the model with example prompts.

## Prerequisites

Before starting, ensure your system meets the following requirements:

1. **Python 3.8+** (Python 3.11 recommended):
   Check your Python version:

   ```bash
   python3 --version
   ```

   Install Python 3.11 (if needed):

   ```bash
   brew install python@3.11
   ```

2. Install  [Mini Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/macos.html#).

### Cloning the Llama2 Repository

   ```bash
   git clone https://github.com/facebookresearch/llama.git
   ```

### Clone the llama C++ port repository

   ```bash
   git clone https://github.com/ggerganov/llama.cpp.git
   ```

Now, both repositories should be in your `llama2` directory.
Inside the `llama.cpp` directory, build it:

   ```bash
   cd llama.cpp
   make
   ```

## Requesting Access to Llama Models

1. Visit [Meta AI Resources](https://ai.meta.com/resources/models-and-libraries/llama-downloads/).
2. Fill in your details in the request form.
3. You’ll receive an email with a unique URL to download the models.

## Downloading the Models

1. In your terminal, navigate to the `llama` directory:

   ```bash
   cd llama
   ```

2. Run the download script:

   ```bash
   /bin/bash ./download.sh
   ```

3. When prompted, enter the custom URL from the email.

## Converting the Downloaded Models

1. **Navigate back to the `llama.cpp` repository**:
   ```bash
   cd llama.cpp
   ```

2. **Create a conda environment named `llama2`**:
   ```bash
   conda create --name llama2
   ```

3. **Activate the environment**:
   ```bash
   conda activate llama2
   ```

4. **Install Python dependencies**:
   ```bash
   python3 -m pip install -r requirements.txt
   ```

5. **Convert the model to f16 format**:
   ```bash
   python3 convert.py --outfile models/7B/ggml-model-f16.bin --outtype f16 ../llama2/llama-2-7b-chat --vocab-dir ../llama2
   ```
   > **Note**: If you encounter an error about a vocab size mismatch (model has -1, but tokenizer.model has 32000), update `params.json` in `../llama2/llama-2-7b-chat` from -1 to 32000.

6. **Quantize the model to reduce its size**:
   ```bash
   ./quantize ./models/7B/ggml-model-f16.bin ./models/7B/ggml-model-q4_0.bin q4_0
   ```

## Running the Model

1. Execute the following command:

   ```bash
   ./main -m ./models/7B/ggml-model-q4_0.bin -n 1024 --repeat_penalty 1.0 --color -i -r "User:" -f ./prompts/chat-with-bob.txt
   ```

   - `-m`: Model file
   - `-n`: Number of tokens
   - `--color`: Colored text input
   - `-i`: Interactive mode
   - `-r "User:"`: User input marker
   - `-f`: Path to prompt file

Now you're ready to use Llama2!
