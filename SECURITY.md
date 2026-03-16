# Security Policy 🛡️

### 🔒 Privacy Statement
SyllabSync processes sensitive data including student emails and calendar events. Since this is a self-hosted or user-managed n8n workflow, your data remains within your n8n instance and the connected Google/Groq services.

### Supported Versions
| Version | Supported          |
| ------- | ------------------ |
| v1.x    | ✅ Active Support   |

### ⚠️ Handling Credentials
**Never** share your `SyllabSync_workflow.json` if it contains:
- Your `groqApiKey`
- Your `studentEmail`
- Active OAuth2 tokens

### Reporting a Vulnerability
If you find a security flaw in how the workflow handles data or authentication, please report it privately:
- **Email:** aditya.bajoria0208@gmail.com
- **Response:** We aim to respond within 48 hours.

### Recommended Best Practices
* Use **Environment Variables** in n8n for your API keys instead of hardcoding them in the Setup node.
* Regularly rotate your **Groq API Key** at [console.groq.com](https://console.groq.com).
