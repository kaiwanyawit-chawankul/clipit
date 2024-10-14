# clipit

Creating a clipboard app that allows users to paste text and get a shareable URL, along with configurable expiration times, usage limits, and admin controls, sounds like a solid project! Using Azure's free services and C#, you can build a scalable app with these core features. Let’s break down the steps and components needed to implement this app.

### Key Features:

1. **Social Login**:
   - Use Azure Active Directory (Azure AD) B2C for authentication via social login providers like Google, Facebook, Microsoft, or custom identity providers.
   - Integration with OAuth 2.0 for social login and authentication.

2. **Text Paste & Sharable URL**:
   - Allow users to paste text into the app, which is then saved and can be retrieved via a generated URL.
   - You can use **Azure Blob Storage** for storing the text content, or **Azure SQL**/Cosmos DB for structured data storage.

3. **Configurable Time-to-Live (TTL) & Expiration**:
   - Users can set a time-to-live (TTL) for their pasted text, after which it will expire (e.g., after 24 hours, or a specific time limit).
   - Alternatively, provide a feature to limit the number of views (e.g., 5 times, then auto-delete).

4. **Admin Controls**:
   - Admin panel to configure settings for the site (e.g., allowed user types, TTL settings, view limits).
   - You can use **Azure App Services** for hosting the web app and **Azure Function App** for serverless back-end logic.

5. **User Types**:
   - Define user types (e.g., Free, Premium) with different restrictions on the number of clips or lifetime of links.

### Architecture Overview:

1. **Frontend (Web App)**:
   - **ASP.NET Core MVC** or **Blazor** (for real-time, interactive UI) as the frontend framework.
   - Integrate with Azure AD B2C for social login and account management.
   - Simple UI for pasting content and retrieving shareable URLs.
   - Manage clipboard content with an interface for setting TTL or view limits.

2. **Backend (API & Logic)**:
   - **ASP.NET Core Web API** or **Azure Functions** to handle business logic (e.g., saving, retrieving, and deleting clipboard data).
   - **Azure SQL** or **Cosmos DB** for structured storage or **Azure Blob Storage** for unstructured storage (storing plain text or JSON objects).
   - Store metadata like TTL, view count, and content details.

3. **Database**:
   - You can use **Azure SQL Database** or **Cosmos DB** for storing clipboard items.
   - Database schema could include:
     - `Id` (unique identifier for the clipboard entry)
     - `Content` (the pasted text)
     - `CreatedAt` (timestamp when the entry was created)
     - `ExpiresAt` (optional TTL setting)
     - `ViewLimit` (number of times the content can be viewed)
     - `Views` (current view count)
     - `UserId` (user who created the clipboard entry)

4. **Azure Blob Storage** (optional):
   - If the text content becomes too large, you might consider using Azure Blob Storage to store the data and store metadata (like TTL, expiry, etc.) in the database.

### Azure Services to Use:
- **Azure Blob Storage** or **Azure SQL**/Cosmos DB for data storage.
- **Azure Functions** or **Web API (ASP.NET Core)** for server-side logic.
- **Azure AD B2C** for authentication and user management.
- **Azure App Services** for hosting the frontend and API.
- **Azure Application Insights** for monitoring and analytics.

---

### Step-by-Step Breakdown:

#### 1. **Setup Authentication (Social Login with Azure AD B2C)**:
   - Configure Azure AD B2C to allow users to sign up or log in with social accounts (like Google, Facebook, etc.).
   - After successful authentication, users are redirected to the main clipboard app.

#### 2. **Frontend - Clipboard Text Submission**:
   - Create a simple web interface (in **ASP.NET Core MVC** or **Blazor**) where users can paste text and configure settings like TTL, view limits, etc.
   - On form submission, send a request to your backend API to save the text data.

#### 3. **Backend - API for Clipboard Data**:
   - **POST** request to save pasted text:
     - Save the content in the database and optionally in Blob Storage (if you want to store larger text).
     - Generate a unique URL for sharing the clipboard content.
   - **GET** request to retrieve and display clipboard content:
     - Check TTL and view limits.
     - If content has expired or exceeded its view limit, return an appropriate response (e.g., 404 or a "Content has expired" message).
   - **PUT** or **DELETE** request to manually delete content (admin or user action).

#### 4. **Admin Dashboard**:
   - Admin panel to manage:
     - User types (Free/Premium) and related restrictions (e.g., number of items, maximum TTL).
     - Global settings for TTL, view limits, and other configurations.
   - **ASP.NET Core MVC** can be used to build the admin interface.

#### 5. **Scalability & Performance**:
   - Use **Azure Functions** to handle background tasks (such as purging expired clipboard entries).
   - Implement a job (using Azure Functions Timer Trigger or Logic Apps) that runs periodically to clean up expired data.
