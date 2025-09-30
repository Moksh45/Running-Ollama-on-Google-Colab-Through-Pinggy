# Running Ollama on Google Colab Through Pinggy

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Moksh45/Running-Ollama-on-Google-Colab-Through-Pinggy/blob/main/Running-Ollama-on-Google-Colab-Through-Pinggy.ipynb)

This repository provides a complete solution for running [Ollama](https://ollama.ai/) (a local LLM inference server) on Google Colab using [Pinggy](https://pinggy.io/) tunneling service. It enables you to serve large language models like Llama 3.2 directly from a Colab notebook and access them via external URLs.

## 🚀 Features

- **Easy Setup**: One-click deployment in Google Colab
- **External Access**: Use Pinggy tunnels to access your Ollama server from anywhere
- **Multiple Interfaces**: Both direct API access and web UI (Open WebUI) support
- **Model Management**: Automated model downloading and serving
- **GPU Support**: Leverages Google Colab's free GPU resources
- **No Configuration**: Pre-configured setup with sensible defaults

## 📋 Prerequisites

- Google account with access to Google Colab
- Basic understanding of Jupyter notebooks
- Internet connection for model downloads

## 🛠️ Getting Started

### Option 1: One-Click Launch (Recommended)
Click the "Open in Colab" badge above to launch the notebook directly in Google Colab.

### Option 2: Manual Setup
1. Clone this repository
2. Upload the `.ipynb` file to Google Colab
3. Run the cells sequentially

## 📖 Usage Instructions

### Step 1: Install Dependencies
The notebook automatically installs:
- `pciutils` (system package)
- `ollama` (LLM inference server)
- `pinggy` (tunneling service)
- `open-webui` (web interface)

### Step 2: Start Ollama Server
```python
import subprocess
def start_ollama_server():
    subprocess.Popen(['ollama', 'serve'])
    print("🚀 Ollama server launched successfully!")
start_ollama_server()
```

### Step 3: Create Pinggy Tunnel for API Access
```python
import pinggy
tunnel1 = pinggy.start_tunnel(
    forwardto="localhost:11434",
    headermodification=["u:Host:localhost:11434"]
)
print(f"Tunnel1 started - URLs: {tunnel1.urls}")
```

### Step 4: Download and Use Models
```python
# Download a model (example: Llama 3.2 1B)
!ollama pull llama3.2:1b

# Make API requests
import requests
response = requests.post(f"{tunnel_url}/api/generate",
                        json={
                            "model": "llama3.2:1b",
                            "prompt": "Hello, how are you?",
                            "stream": False
                        })
print(response.json()["response"])
```

### Step 5: Set Up Web UI (Optional)
```python
# Create tunnel for Open WebUI
tunnel2 = pinggy.start_tunnel(forwardto="localhost:8000")
print(f"WebUI URLs: {tunnel2.urls}")

# Start Open WebUI server
!open-webui serve --port 8000
```

## 🔗 API Endpoints

Once your Ollama server is running through Pinggy, you can access these endpoints:

- **Generate Text**: `POST /api/generate`
- **Chat Completion**: `POST /api/chat`
- **List Models**: `GET /api/tags`
- **Model Information**: `POST /api/show`

### Example API Usage

```python
import requests
import json

# Your Pinggy tunnel URL
tunnel_url = "https://your-tunnel-id.a.free.pinggy.link"

# Generate text
response = requests.post(f"{tunnel_url}/api/generate", 
    json={
        "model": "llama3.2:1b",
        "prompt": "Explain quantum computing in simple terms",
        "stream": False
    }
)

print(response.json()["response"])
```

```bash
# Using curl
curl -X POST https://your-tunnel-id.a.free.pinggy.link/api/generate \
  -H "Content-Type: application/json" \
  -d '{
    "model": "llama3.2:1b",
    "prompt": "What is machine learning?",
    "stream": false
  }'
```

## 🖥️ Web Interface

Open WebUI provides a ChatGPT-like interface for interacting with your models:

1. After running the WebUI setup cells, you'll get a Pinggy URL
2. Open the URL in your browser
3. Create an account or sign in
4. Start chatting with your models

## 🔧 Troubleshooting

### Common Issues

**Ollama server not starting:**
- Ensure the Ollama installation cell completed successfully
- Check if port 11434 is available: Run the port check cell
- Restart the server if needed

**Pinggy tunnel not working:**
- Verify your internet connection
- Try regenerating the tunnel
- Check if the local service is running

**Model download fails:**
- Ensure sufficient disk space in Colab
- Check internet connectivity
- Try downloading a smaller model first

**Open WebUI not accessible:**
- Verify the WebUI server started successfully
- Check the Pinggy tunnel URL is correct
- Ensure port 8000 is properly forwarded

### Debug Commands

```python
# Check if Ollama is running
!ps aux | grep ollama

# Check port usage
!sudo lsof -i :11434
!sudo lsof -i :8000

# Test local connectivity
!curl http://localhost:11434/api/tags
```

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

### Areas for Contribution

- Additional model configurations
- Enhanced error handling
- UI improvements
- Documentation updates
- Performance optimizations

## 📚 Additional Resources

- [Ollama Documentation](https://ollama.ai/docs)
- [Pinggy Documentation](https://pinggy.io/docs)
- [Open WebUI Documentation](https://docs.openwebui.com/)
- [Google Colab Documentation](https://colab.research.google.com/notebooks/intro.ipynb)

## ⚠️ Important Notes

- **Resource Limits**: Google Colab has usage limits. Monitor your resource consumption.
- **Data Persistence**: Colab instances are temporary. Downloaded models will be lost when the session ends.
- **Security**: Pinggy tunnels are public. Avoid sharing sensitive information.
- **Model Size**: Larger models may exceed Colab's memory limits. Start with smaller models.

## 🏷️ Supported Models

This setup works with any Ollama-compatible model. Popular choices include:

- `llama3.2:1b` (recommended for Colab free tier)
- `llama3.2:3b`
- `gemma:2b`
- `phi3:mini`
- `codellama:7b`

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🙏 Acknowledgments

- [Ollama Team](https://ollama.ai/) for the excellent LLM inference server
- [Pinggy](https://pinggy.io/) for providing tunneling services
- [Open WebUI](https://openwebui.com/) for the web interface
- Google Colab for providing free GPU resources

---

**⭐ If this project helped you, please consider giving it a star!**
