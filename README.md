# Google Calendar MCP Server

This is a Model Context Protocol (MCP) server that provides integration with Google Calendar. It allows LLMs to read, create, and manage calendar events through a standardized interface.

## Features

- List available calendars
- List events from a calendar
- Create new calendar events
- Update existing events
- Delete events
- Process events from screenshots and images

## Requirements

- Node.js 16 or higher
- TypeScript 5.3 or higher
- A Google Cloud project with the Calendar API enabled
- OAuth 2.0 credentials (Client ID and Client Secret)

## Project Structure

```
google-calendar-mcp/
├── src/           # TypeScript source files
├── build/         # Compiled JavaScript output
├── llm/           # LLM-specific configurations and prompts
├── package.json   # Project dependencies and scripts
└── tsconfig.json  # TypeScript configuration
```

## Google Cloud Setup

1. Go to the [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project or select an existing one.
3. Enable the [Google Calendar API](https://console.cloud.google.com/apis/library/calendar-json.googleapis.com).
4. Create OAuth 2.0 credentials:
   - Go to **Credentials**
   - Click **"Create Credentials" > "OAuth client ID"**
   - Choose **"User data"** as the type of data the app will be accessing.
   - Add your app name and contact information.
   - Add the following scope (optional):
     - `https://www.googleapis.com/auth/calendar.events`
   - Select **"Desktop app"** as the application type.
   - Add your email address as a test user under the [OAuth Consent screen](https://console.cloud.google.com/apis/credentials/consent).
     - **Note:** It may take a few minutes for the test user to propagate.

## Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/pashpashpash/google-calendar-mcp.git
   cd google-calendar-mcp
   ```
2. Install dependencies:
   ```sh
   npm install
   ```
3. Build the TypeScript code:
   ```sh
   npm run build
   ```
4. Download your Google OAuth credentials from the Google Cloud Console.
   - Rename the file to `gcp-oauth.keys.json`
   - Place it in the root directory of the project.

5. Run the server:
   ```sh
   node build/index.js
   ```

## Available Scripts

- `npm run build` - Build the TypeScript code.
- `npm run build:watch` - Build TypeScript in watch mode for development.
- `npm run dev` - Start the server in development mode using ts-node.
- `npm run auth` - Start the authentication server for Google OAuth flow.

## Authentication Setup

### Automatic Authentication (Recommended)

1. Ensure your OAuth credentials are in `gcp-oauth.keys.json`
2. Start the MCP server:
   ```sh
   npm start
   ```
3. If no authentication tokens are found, the server will:
   - Start an authentication server (on ports 3000-3004).
   - Open a **browser window** for OAuth authentication.
   - Save the authentication tokens securely.
   - Shut down the authentication server and continue normal operation.

### Manual Authentication

If you prefer to **manually authenticate**, run:
```sh
npm run auth
```
- This starts an authentication server, opens a browser for OAuth, and saves the tokens.

### Security Notes

- OAuth credentials are stored in `gcp-oauth.keys.json`
- Authentication tokens are stored in `.gcp-saved-tokens.json` with 600 permissions.
- Tokens **refresh automatically** before expiration.
- If token refresh fails, you’ll be prompted to re-authenticate.
- **Never commit OAuth credentials or token files to version control.**

## Usage

The server provides the following tools:

| Tool            | Description |
|----------------|-------------|
| `list-calendars` | List all available calendars |
| `list-events`   | List events from a calendar |
| `create-event`  | Create a new calendar event |
| `update-event`  | Update an existing calendar event |
| `delete-event`  | Delete a calendar event |

## Using with Claude Desktop

1. Modify your Claude Desktop config file (e.g., `/Users/<user>/Library/Application Support/Claude/claude_desktop_config.json`):
   ```json
   {
     "mcpServers": {
       "google-calendar": {
         "command": "node",
         "args": ["path/to/build/index.js"]
       }
     }
   }
   ```
2. Restart Claude Desktop.

## Example Use Cases

### 📅 Add events from screenshots and images
```
Add this event to my calendar based on the attached screenshot.
```
✅ **Supported formats**: PNG, JPEG, GIF  
✅ Extracts details like **date, time, location, description**  

### 🔎 Check attendance
```
Which events tomorrow have attendees who haven't accepted the invitation?
```

### 🤖 Auto-schedule meetings
```
Here's availability from someone I'm interviewing. Find a time that works on my work calendar.
```

### 📆 Find free time across calendars
```
Show my available time slots for next week. Consider both my personal and work calendar.
```

## Troubleshooting

| Issue                        | Solution |
|------------------------------|-------------|
| OAuth token expires after 7 days | You must **re-authenticate** if the app is in testing mode. |
| OAuth token errors | Ensure `gcp-oauth.keys.json` is formatted correctly. |
| TypeScript build errors | Run `npm install` and `npm run build`. |
| Image processing issues | Ensure the image format is **PNG, JPEG, or GIF**. |

## Security Notes

- The server runs **locally** and requires **OAuth authentication**.
- OAuth credentials must be stored in `gcp-oauth.keys.json` in the project root.
- **Tokens refresh automatically** when expired.
- **DO NOT** commit credentials or tokens to version control.
- For **production use**, get your OAuth app verified by Google.

## License

This project is licensed under the **MIT License**. See the [LICENSE](LICENSE) file for details.

## Contributing

Want to contribute?

1. Fork the repository.
2. Create a new branch:
   ```sh
   git checkout -b feature-branch
   ```
3. Make changes & commit:
   ```sh
   git commit -m "Added new feature"
   ```
4. Push and open a **pull request**:
   ```sh
   git push origin feature-branch
   ```

## Attribution

This project is a fork of the original **[nspady/google-calendar-mcp](https://github.com/nspady/google-calendar-mcp)** repository.

## Stay Updated

🔗 **[GitHub: pashpashpash/google-calendar-mcp](https://github.com/pashpashpash/google-calendar-mcp)**

---

### TL;DR Setup
```sh
git clone https://github.com/pashpashpash/google-calendar-mcp.git
cd google-calendar-mcp
npm install
npm run build
node build/index.js
```
Then **connect your Notion integration and you're good to go! 🚀**
