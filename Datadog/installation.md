
## 📊 Datadog Agent Setup (Step-by-Step)

### 🔹 1. Create Datadog Account & Get API Key

1. Go to 👉 [https://www.datadoghq.com](https://www.datadoghq.com)
2. Sign up / Login
3. Navigate to:
   **Organization Settings → API Keys**
4. Copy your API Key

---

### 🔹 2. Set API Key (Environment Variable)

```bash
export DD_API_KEY="YOUR_API_KEY"
```

---

### 🔹 3. Install Datadog Agent (v7)

```bash
DD_AGENT_MAJOR_VERSION=7 DD_API_KEY=$DD_API_KEY bash -c "$(curl -L https://s3.amazonaws.com/dd-agent/scripts/install_script.sh)"
```

---

### 🔹 4. Verify Agent Status

```bash
sudo systemctl status datadog-agent
sudo datadog-agent status
```

---

### 🔹 5. Configure Environment File

Edit environment file:

```bash
sudo vim /etc/datadog-agent/environment
```

Add:

```bash
DD_API_KEY=your_api_key_here
DD_SITE=ap1.datadoghq.com
```

---

### 🔹 6. Update Datadog Config File

```bash
sudo vim /etc/datadog-agent/datadog.yaml
```

Update API key:

```yaml
api_key: your_api_key_here
```

---

### 🔹 7. Restart Datadog Agent

```bash
sudo systemctl restart datadog-agent
```

---

### 🔹 8. Final Verification

```bash
sudo datadog-agent status
```

Check API key:

```bash
sudo datadog-agent status | grep "API Key"
```

---

## ✅ Expected Output

You should see something like:

```
API Key ending with: xxxx
```

---

## 💡 Tips

* Always use **correct site** (`ap1.datadoghq.com` for Asia Pacific)
* Keep your API key **secure** (never push to GitHub 🚫)
* If agent not working → restart + check logs:

```bash
sudo journalctl -u datadog-agent
```

