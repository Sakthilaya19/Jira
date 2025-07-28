import requests
from requests.auth import HTTPBasicAuth

# Replace with your actual JIRA details
JIRA_DOMAIN = "https://sakthilayab.atlassian.net"
JIRA_USER = "your-email@example.com"          # ✅ Replace this
JIRA_API_TOKEN = "your-jira-api-token"        # ✅ Replace this
ISSUE_KEY = "CS-123"                           # ✅ Replace this with your JIRA issue key
FILENAME = "jira_rag_report.txt"               # or "jira_rag_report.pdf"

# Authentication
auth = HTTPBasicAuth(JIRA_USER, JIRA_API_TOKEN)

# URL to attach file
url = f"{JIRA_DOMAIN}/rest/api/2/issue/{ISSUE_KEY}/attachments"

headers = {
    "X-Atlassian-Token": "no-check"  # Required for file upload
}

files = {
    'file': open(FILENAME, 'rb')
}

# Make POST request to attach file
response = requests.post(
    url,
    headers=headers,
    auth=auth,
    files=files
)

# ✅ Check response
if response.status_code == 200 or response.status_code == 201:
    print(f"✅ Successfully attached '{FILENAME}' to issue {ISSUE_KEY}")
else:
    print(f"❌ Failed to attach file. Status Code: {response.status_code}")
    print("Response:", response.text)
