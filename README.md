# 🎬 Movie Alert

A web application that allows users to set up movie alerts for their preferred theatres. Users can specify which movies they want to watch and select their preferred cinemas to receive notifications.

## 📋 Project Overview

**Movie Alert** is a full-stack web application built with:
- **Frontend**: HTML5, CSS3, JavaScript (Vanilla JS)
- **Backend**: Zoho Catalyst (serverless functions)
- **Authentication**: Zoho Catalyst Auth SDK
- **UI Framework**: Bootstrap 5
- **UI Components**: SweetAlert2 for notifications
- **HTTP Client**: jQuery for AJAX requests

## 🎯 Features

### 1. **User Authentication**
- Secure login/logout functionality using Zoho Catalyst Auth SDK
- Users must authenticate before accessing the movie alert dashboard
- Session management with email verification

### 2. **Movie Alert Management**
- Users can enter a movie name and create alerts
- Save movie preferences to the backend database
- Retrieve and load previously saved preferences
- Delete or update existing movie alerts

### 3. **Theatre Selection**
- **Two Selection Modes**:
  - **Preferred Theatres**: Select specific theatres you want to monitor (multi-select dropdown)
  - **All Theatres**: Monitor all available theatres
- Dynamic theatre list loaded from `theatres.json`
- Searchable dropdown for easy theatre discovery

### 4. **Persistent Data Storage**
- All preferences are stored in Zoho Catalyst datastore
- Previously saved preferences automatically load when you enter a movie name
- Theatre selections are preserved across sessions

## 📁 Project Structure

```
Movie-alert/
├── index.html              # Main application page (dashboard)
├── main.js                 # Core application logic
├── main.css                # Application styles
├── styles.css              # Additional styles
├── theatres.json           # List of available theatres
├── Auth/
│   └── index.html          # Authentication login page
└── .gitignore              # Git ignore rules
```

## 🔧 Files Explanation

### **index.html** - Main Dashboard
- Displays the movie alert form
- Contains the welcome message showing logged-in user email
- Radio buttons to switch between "Preferred Theatres" and "All Theatres" modes
- Dropdown multiselect for theatre selection
- Submit button to create movie alerts
- Logout button in the top-right corner

### **main.js** - Core Application Logic

#### Key Variables:
- `userEmail`: Stores the logged-in user's email address
- `SERVER_URL`: Catalyst serverless function endpoint for datastore operations

#### Main Functions:

**Authentication & Initialization** (`DOMContentLoaded` event):
- Verifies user authentication using Catalyst SDK
- Redirects to login page if user is not authenticated
- Loads theatre list from `theatres.json`
- Initializes dropdown and event listeners

**Theatre Management**:
- `loadTheatres()`: Fetches theatre list from JSON file
- `toggleMultiselect()`: Shows/hides theatre selector based on radio button selection
- `updateDropdownLabel()`: Updates dropdown button text to show selected theatres

**Data Operations**:
- `fetchExistingMoviePreference(movie)`: GET request to retrieve previously saved preferences
- `populatePreviousSelection(data)`: Populates form with saved theatre selections and reorders options with saved theatres at the top
- `postAlienEncounter()`: POST request to submit a new movie alert with selected theatres

**UI Interactions**:
- Theatre dropdown toggle and click-outside handling
- Theatre search filter for quick selection
- Form validation (movie name required, theatres required if preferred mode selected)

**Session Management**:
- `logout()`: Signs out user and redirects to login page
- `resetTheatreSelectionToDefault()`: Resets form to initial state

### **Auth/index.html** - Login Page
- Renders Zoho Catalyst authentication widget
- Redirects authenticated users to `/index.html` (main dashboard)
- Responsive gradient background

### **theatres.json** - Theatre Database
```json
[
  "Theatre Name 1",
  "Theatre Name 2",
  "Theatre Name 3",
  ...
]
```
- Array of available cinema/theatre names
- Used to populate the theatre selection dropdown

### **main.css & styles.css**
- Custom styling for the application
- Responsive design for mobile and desktop
- Dropdown and form styling

## 🔄 Data Flow

### 1. **User Login**
```
User → Auth Page → Catalyst Auth → Authenticated → Dashboard
```

### 2. **Create Movie Alert**
```
User enters movie name → 
Selects theatres (optional) → 
Clicks Submit → 
AJAX POST to SERVER_URL → 
Data stored in Catalyst datastore → 
Success notification
```

### 3. **Load Existing Preference**
```
User enters movie name → 
Focus blur event → 
AJAX GET to SERVER_URL → 
Check if movie exists → 
If exists: Load and display saved theatres → 
Reorder UI with saved theatres at top
```

## 🔌 API Integration

### Catalyst Serverless Function
**Endpoint**: `https://movie-alert-60047185658.development.catalystserverless.in/server/movie_alert/datastore`

**POST Request** (Create/Update):
```javascript
{
  "Movie_name": "Avengers Endgame",
  "Emails": "user@example.com",
  "isPreferredTheatre": true,
  "preferredTheatres": "Theatre1|Theatre2|Theatre3"
}
```

**GET Request** (Fetch):
```
?Movie_name=Avengers Endgame&Emails=user@example.com
```

**Response**:
```javascript
{
  "status": "exists",
  "data": {
    "isPreferredTheatre": true,
    "preferredTheatres": "Theatre1|Theatre2"
  }
}
```

## 🎨 UI Components

### Dropdown Multiselect
- Click-outside detection to close dropdown
- Search filter to find theatres quickly
- Reordering: previously selected theatres appear at the top
- Scroll to top on dropdown open
- Updates button text based on selection count

### Form Validation
- Movie name is required
- If "Preferred Theatres" mode is selected, at least one theatre must be chosen
- Displays SweetAlert2 notifications for errors and success messages

## �� Technologies Used

| Technology | Purpose |
|-----------|---------|
| **Zoho Catalyst** | Serverless backend, authentication, database |
| **Bootstrap 5** | Responsive UI framework |
| **SweetAlert2** | User notifications and alerts |
| **jQuery** | AJAX requests and DOM manipulation |
| **Vanilla JavaScript** | Core application logic |

## 🔐 Security Features

- User authentication via Zoho Catalyst
- Email-based user identification
- Data tied to user email (multi-tenant isolation)
- No sensitive data stored in frontend code

## 📝 Usage Instructions

1. **Login**: Navigate to the app and authenticate with your Zoho account
2. **Enter Movie**: Type the movie name in the input field
3. **Select Mode**: Choose between "Preferred Theatres" or "All Theatres"
4. **Select Theatres** (if preferred): Click the dropdown and select one or more theatres
5. **Submit**: Click the "Submit" button to save your movie alert
6. **View Saved**: Enter the same movie name again to see your previously saved preferences

## 🔗 Dependencies

- Zoho Catalyst SDK (v4.6.1)
- Bootstrap 5.3.2
- SweetAlert2 v11
- jQuery 3.4.1

## 📦 Installation & Setup

1. Clone this repository
2. Ensure you have Zoho Catalyst account configured
3. Update `SERVER_URL` in `main.js` with your actual Catalyst endpoint
4. Host on Zoho Catalyst or any web server
5. Update `theatres.json` with your actual theatre list

## �� Known Limitations

- Theatre list must be manually updated in `theatres.json`
- No edit functionality for existing alerts (only create and view)
- Single theatre selection mode (Preferred Theatres shows all selected theatres with "|" separator)

## 🎓 Code Highlights

### Event Delegation Pattern
```javascript
document.addEventListener("DOMContentLoaded", async function () {
  // Initialize app
});
```

### Async Theatre Loading
```javascript
const theatreList = await (async () => {
  // Fetch from JSON or use default
})();
```

### Dynamic DOM Cloning
```javascript
const clone = template.content.cloneNode(true);
// Populate and append cloned template
```

### AJAX Request Pattern
```javascript
$.ajax({
  url: SERVER_URL,
  type: "GET" | "POST",
  success: function(res) { },
  error: function() { }
});
```

## 👥 Author

Created as a web application for movie theatre alert notifications.

## 📄 License

This project is part of the Movie Alert system.

---

**Last Updated**: January 7, 2026
