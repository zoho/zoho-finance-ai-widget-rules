# Vue.js Widget Development Rules

## Overview

This document defines the rules and best practices for creating Zoho Finance widgets using **Vue.js 2.x** with the Options API. All Vue widgets must follow the architecture patterns established in this codebase.

---

## Project Structure

### Vue Folder Structure

**CRITICAL RULE: Framework Folder Structure**

When creating Vue widgets:

1. **ALWAYS use the existing Vue folder if it exists** (e.g., `samplevueapp`)
   - **Overwrite/update code in the existing Vue folder**
   - Do NOT create a new folder if one already exists

2. **If Vue folder doesn't exist**, create it using Vue CLI:
   - `vue create samplevueapp` (or `vue create <folder_name>`)

3. **Code Location:**
   - **Write ALL Vue code in the Vue folder** (`samplevueapp/src/`)
   - **Do NOT write Vue code in widget folder** (`samplevueappWidget/`)
   - **Do NOT create webpack config in widget folder**
   - **Do NOT use webpack in widget folder** - Vue build tools handle compilation

4. **Pure JavaScript / Vanilla JS:**
   - **Only write in widget folder when NO framework is mentioned**
   - If framework is NOT mentioned → Write directly in `samplevueappWidget/app/js/`
   - If Vue is mentioned → Write in Vue folder (`samplevueapp/src/`)

### Vue Source Code Structure

Vue widgets must follow this directory structure within the Vue folder:

```
samplevueapp/
├── src/
│   ├── main.js                    # Entry point with ZFAPPS initialization
│   ├── App.vue                    # Root component
│   ├── router/
│   │   └── index.js               # Vue Router configuration
│   ├── views/                     # Page-level components (widget views)
│   │   ├── Services.vue
│   │   └── UnpaidAppointment.vue
│   ├── components/                # Reusable UI components
│   │   ├── button.vue
│   │   ├── pagination.vue
│   │   ├── search.vue
│   │   └── ...
│   ├── services/                  # Business logic layer (API operations)
│   │   ├── bookingService.js
│   │   ├── financeService.js
│   │   └── invoiceService.js
│   ├── mixins/                    # Reusable Vue mixins
│   │   ├── paginationMixin.js
│   │   ├── searchMixin.js
│   │   └── tableMixin.js
│   └── utils/                     # Utility functions and constants
│       ├── api.js                 # Base API class
│       ├── constants.js           # Application constants
│       ├── notification.js        # Notification utilities
│       └── properties.js          # Static properties/config
├── public/
│   └── index.html                 # Main HTML (MUST include Zoho SDK script)
├── vue.config.js                  # Vue build configuration (base path: ".")
└── package.json
```

### Decision Tree:

```
Is Vue framework mentioned?
├─ YES → Does Vue folder exist?
│   ├─ YES → Use existing folder, overwrite/update code in it
│   └─ NO → Create folder using `vue create <folder_name>`, then write code
│
└─ NO (Pure JS/Vanilla JS) → Write directly in widget folder
```

### Widget Folder Detection

**CRITICAL:** Before creating any widget folder, always check if one already exists:

1. **Check for existing widget folders:**
   - Search for `plugin-manifest.json` files in the workspace
   - If found, a widget folder already exists

2. **If widget folder exists:**
   - **ASK the user:** "I found an existing widget folder [folder_name]. Should I add the widget to this existing folder?"
   - **If user says YES:** Add widget entry to existing `plugin-manifest.json`
   - **If user says NO:** Do NOT create a new widget folder, only keep Vue code in Vue folder

3. **If NO widget folder exists:**
   - Do NOT create widget folder

**Important:**
- **NEVER create widget folder without asking the user first**
- **ALWAYS check for existing widget folders by searching for `plugin-manifest.json`**
- Widget folder is only needed for `plugin-manifest.json` and widget.html

---

### Plugin Manifest URL Configuration

**CRITICAL:** When Vue folder exists next to widget folder (e.g., `samplevueapp/` next to `samplevueappWidget/`):

1. **Use localhost URL in plugin-manifest.json:**
   - Format: `https://localhost:8080#/route-path` (or appropriate port)
   - Example: `https://localhost:8080#/email-input`
   - Example: `https://localhost:8080` (for root route)

2. **Hash Routing:**
   - Use hash (`#`) for Vue Router routes
   - Root route: `https://localhost:8080` or `https://localhost:8080#/`
   - Other routes: `https://localhost:8080#/route-name`

3. **No Widget Folder Changes:**
   - **Do NOT edit widget.html or other files in widget folder**
   - **Only update plugin-manifest.json** with localhost URL
   - Widget folder remains unchanged

4. **Development vs Production:**
   - **Development:** Use `https://localhost:8080#/route` (Vue dev server)
   - **Production:** Use `/app/widget.html#/route` (built files)

**Example plugin-manifest.json:**

```json
{
  "modules": {
    "widgets": [
      {
        "location": "customer.creation.sidebar",
        "name": "Customer Email Input",
        "url": "https://localhost:8080#/email-input",
        "logo": "/app/img/logo.png"
      },
      {
        "location": "customer.details.sidebar",
        "name": "Customer Sales Overview",
        "url": "https://localhost:8080",
        "logo": "/app/img/logo.png"
      },
      {
        "location": "invoice.details.sidebar",
        "name": "Invoice Customer Details",
        "url": "https://localhost:8081",
        "logo": "/app/img/logo.png"
      }
    ]
  }
}
```

**Important:**
- Only change `plugin-manifest.json` - no other widget folder files
- Use localhost URL when Vue folder exists for development
- Port number may vary (8080, 8081, etc.) - use the port where Vue dev server runs

---

## Zoho SDK Script Requirement

### Index.html / Main HTML

**CRITICAL:** The Vue app's main HTML file (`public/index.html` or root `index.html`) **MUST** include the Zoho Finance SDK script before the Vue app script:

```html
<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Vue Widget</title>
</head>
<body>
  <div id="app"></div>
  
  <!-- Zoho Finance SDK - MUST be included first -->
  <script src="https://js.zohostatic.com/zohofinance/v1/zf_sdk.js"></script>
  
  <!-- Vue app script (built) -->
  <script src="./js/widget.js"></script>
</body>
</html>
```

**Important:**
- The Zoho SDK script **MUST** be loaded before the Vue app script
- Use the exact URL: `https://js.zohostatic.com/zohofinance/v1/zf_sdk.js`
- This ensures `window.ZFAPPS` is available when Vue initializes

---

## Vue Build Configuration

### Base Path Configuration

**CRITICAL:** Vue build configuration (`vue.config.js` or `vite.config.js`) **MUST** set the base path to start with `"."`:

#### For Vue CLI (vue.config.js):

```javascript
const { defineConfig } = require('@vue/cli-service')

module.exports = defineConfig({
  transpileDependencies: true,
  publicPath: '.',  // Base path MUST start with "."
  filenameHashing: false,  // Disable hash in filenames
  devServer: {
    https: true  // REQUIRED: Enable HTTPS for development server
  },
  chainWebpack: config => {
    // Disable code splitting for widget (creates single bundle)
    config.optimization.delete('splitChunks')
  }
})
```

#### For Vite (vite.config.js):

```javascript
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

export default defineConfig({
  plugins: [vue()],
  base: '.',  // Base path MUST start with "."
  server: {
    https: true  // REQUIRED: Enable HTTPS for development server
  },
  build: {
    // Use default dist folder - do NOT set outDir
  }
})
```

**Important:**
- Base path (`publicPath` or `base`) **MUST** be set to `"."`
- This ensures relative paths work correctly in the widget environment
- **Do NOT set `outputDir` or `outDir`** - Use default `dist` folder
- **MUST configure devServer/server with `https: true`** for development
- Build output goes to default location: `samplevueapp/dist/` (Vue CLI) or `dist/` (Vite)
- After building, copy built files from `dist/` to widget folder as needed

---

## Initialization Pattern

### Main.js Structure

All Vue widgets must initialize ZFAPPS in `main.js` before mounting the Vue app:

```javascript
import Vue from 'vue';
import App from './App.vue';
import router from './router';
import { Api } from './utils/api';
import { CONNECTION_NAMES, ORG_VARIABLE } from './utils/constants';

Vue.config.productionTip = false;

async function initializeApp() {
  if (window.ZFAPPS) {
    window.ZFAPPS.extension.init().then(async (zapp) => {
      const organizationMap = await window.ZFAPPS.get('organization');
      const { invoice: { line_items } } = await window.ZFAPPS.get('invoice');
      const { organization: { api_root_endpoint } } = organizationMap;
      const url = new URL(api_root_endpoint);
      const apiRoot = url.origin;
      
      window.zapp = zapp;
      
      // Store app metadata globally
      Vue.prototype.$appMeta = {
        organizationMap: JSON.stringify(organizationMap),
        line_items: JSON.stringify(line_items),
        orgVariable: ORG_VARIABLE,
        financeApi: new Api(`${api_root_endpoint}`, `${CONNECTION_NAMES.FINANCE}`),
        bookingApi: `${apiRoot}/bookings/v1/json`,
        financeConnection: CONNECTION_NAMES.FINANCE,
        bookingConnection: CONNECTION_NAMES.BOOKING
      };
      
      new Vue({
        zapp: zapp,
        router,
        ZFAPPS: window.ZFAPPS,
        render: h => h(App)
      }).$mount('#app');
    });
  }
}

initializeApp();
```

**Important:**
- Always check for `window.ZFAPPS` before initialization
- Store app metadata in `Vue.prototype.$appMeta` for global access
- Use constants from `utils/constants.js` instead of hardcoded strings
- Initialize services after ZFAPPS is ready

---

## Service Layer Pattern

### Creating Services

All API operations must be encapsulated in service classes located in `src/services/`. Services handle:
- API request/response logic
- Error handling
- Data transformation
- Business logic

### Service Structure

```javascript
import Notification from '../utils/notification';

/**
 * Service Name
 * Brief description of what this service handles
 */
class ServiceName {
  constructor(dependency1, dependency2) {
    this.dependency1 = dependency1;
    this.dependency2 = dependency2;
  }

  /**
   * Make a request to the API
   * @param {string} endpoint - API endpoint
   * @param {string} method - HTTP method
   * @param {object} body - Request body (optional)
   * @returns {Promise<object>} API response
   */
  async makeRequest(endpoint, method, body = '') {
    try {
      // API call logic
      const response = await window.ZFAPPS.request({
        url: endpoint,
        method: method,
        connection_link_name: this.connectionName,
        body: body ? {
          mode: 'raw',
          raw: JSON.stringify(body)
        } : undefined
      });

      const parsedResponse = JSON.parse(response.data.body);
      
      if (parsedResponse.code) {
        await Notification.showErrorsMsg(parsedResponse.message);
        return null;
      }
      
      return parsedResponse;
    } catch (error) {
      await Notification.showErrorsMsg(error.message);
      return null;
    }
  }

  /**
   * Specific business method
   * @param {string} param - Parameter description
   * @returns {Promise<Array>} Result description
   */
  async fetchData(param) {
    // Implementation
  }
}

export default ServiceName;
```

### Service Usage in Components

```javascript
// In component's created() hook
async created() {
  // Initialize service
  this.myService = new MyService(
    this.$appMeta.apiEndpoint,
    this.$appMeta.connectionName
  );

  // Use service
  const data = await this.myService.fetchData();
}
```

**Rules:**
- **NEVER** make direct API calls in Vue components
- **ALWAYS** use services for API operations
- Services must handle errors internally and return null on failure
- Use JSDoc comments for all public methods
- Services should be stateless (no internal state)

---

## Mixin Pattern

### Creating Mixins

Reusable component logic must be extracted into mixins located in `src/mixins/`. Mixins provide:
- Shared data properties
- Shared methods
- Shared lifecycle hooks

### Mixin Structure

```javascript
/**
 * Mixin Name
 * Description of what this mixin provides
 */
export default {
  data() {
    return {
      // Shared data properties
      sharedProperty: null
    };
  },

  methods: {
    /**
     * Shared method
     * @param {type} param - Parameter description
     */
    sharedMethod(param) {
      // Implementation
    }
  },

  // Optional: shared lifecycle hooks
  created() {
    // Shared initialization
  }
};
```

### Using Mixins in Components

```javascript
import myMixin from '../mixins/myMixin';

export default {
  name: 'MyComponent',
  mixins: [myMixin],
  // Component-specific code
};
```

**Available Mixins:**
- `paginationMixin` - Pagination logic (currentPage, itemsPerPage, showTable, perPageSelected)
- `searchMixin` - Search functionality (searchArrayList, handlesearchArray)
- `tableMixin` - Table operations (selectedList, createTable, selectedArray)

**Rules:**
- Use mixins for logic shared across multiple components
- Keep mixins focused on a single concern
- Document all mixin properties and methods
- Components can override mixin methods if needed

---

## Component Structure

### View Components (Pages)

View components in `src/views/` should:
- Focus on UI rendering
- Use services for data fetching
- Use mixins for shared functionality
- Keep business logic minimal

### View Component Template

```vue
<template>
  <div class="widget-container">
    <!-- Search Section -->
    <div class="search-section">
      <search-text 
        :placeholder="placeholder" 
        :tableList="searchArrayList" 
        :displayKeyValue="searchcolName" 
        @searched-Array="handlesearchArray"
      />
    </div>

    <!-- Table Section -->
    <div class="table-container">
      <list-view 
        :items="createTableList" 
        :itemHeading="headerName" 
        @update:selectedIndices="selectedArray" 
        :loading="isloading"
      />
    </div>

    <!-- Pagination Section -->
    <div class="pagination-container" v-if="totalItems >= 10">
      <perpage 
        v-on:templateContentUpdate="perPageSelected" 
        :predefinedSelectedValue="perPagePreDefinedValue" 
        :templateData="perpageArray"
      />
      <pagination 
        :totalItems="totalItems" 
        :itemsPerPage="itemsPerPage" 
        :currentPage="currentPage" 
        @update:items_page="showTable" 
      />
    </div>

    <!-- Footer Actions -->
    <footer class="footer">
      <buttonvue 
        :buttonText="button1" 
        :isSpin="isspin" 
        :isdisabled="isdisabled" 
        @button-click="handleEvent"
      />
    </footer>
  </div>
</template>

<script>
import searchText from '../components/search.vue';
import listView from '../components/listView.vue';
import pagination from '../components/pagination.vue';
import perpage from '../components/perpage.vue';
import buttonvue from '../components/button.vue';
import MyService from '../services/myService';
import paginationMixin from '../mixins/paginationMixin';
import searchMixin from '../mixins/searchMixin';
import tableMixin from '../mixins/tableMixin';
import { MODAL_SIZE } from '../utils/constants';

export default {
  name: 'MyView',
  mixins: [paginationMixin, searchMixin, tableMixin],
  data() {
    return {
      // Component-specific data only
      headerName: [],
      myService: null
    };
  },
  components: {
    searchText,
    listView,
    pagination,
    perpage,
    buttonvue
  },
  methods: {
    /**
     * Handle primary action
     */
    async handleEvent() {
      this.isspin = true;
      this.isdisabled = true;
      
      // Use service for business logic
      const success = await this.myService.performAction(this.selectedList);
      
      if (success) {
        window.ZFAPPS.closeModal();
      }
      
      this.isspin = false;
      this.isdisabled = false;
    }
  },
  async created() {
    // Initialize services
    this.myService = new MyService(
      this.$appMeta.apiEndpoint,
      this.$appMeta.connectionName
    );

    // Resize modal if needed
    window.ZFAPPS.invoke('RESIZE', { 
      width: MODAL_SIZE.WIDTH, 
      height: MODAL_SIZE.HEIGHT 
    });

    // Fetch initial data
    const data = await this.myService.fetchData();
    
    // Initialize pagination, search, and table
    this.initializePagination(data);
    this.initializeSearch(this.itemArray);
    await this.createTable(this.itemArray);
  }
};
</script>
```

**Rules:**
- **NEVER** duplicate code between view components
- **ALWAYS** use services for API calls
- **ALWAYS** use mixins for shared functionality
- Keep component data minimal (only component-specific properties)
- Use JSDoc comments for methods
- Initialize services in `created()` hook

---

## Constants Usage

### Creating Constants

All magic strings and configuration values must be defined in `src/utils/constants.js`:

```javascript
export const CONNECTION_NAMES = {
  FINANCE: 'bookings_finance_connection',
  BOOKING: 'booking_connection'
};

export const ORG_VARIABLE = 'vl__com_x9l9bu_integration_status';

export const MODAL_SIZE = {
  WIDTH: '920px',
  HEIGHT: '750px'
};

export const DEFAULT_PAGINATION = {
  PER_PAGE: 25,
  PER_PAGE_OPTIONS: [
    { name: '10 per page', value: 10 },
    { name: '25 per page', value: 25 }
  ]
};
```

**Rules:**
- **NEVER** use hardcoded strings in components/services
- **ALWAYS** import constants from `utils/constants.js`
- Group related constants in objects
- Use descriptive constant names

---

## Error Handling

### Notification Utility

**Important:** Use the Notification utility for all user-facing messages. **Do NOT create separate components** to show error or success messages - the Notification SDK handles this automatically using Zoho's built-in notification system.

For detailed Notification SDK usage, refer to [SDK Rules - Showing Notifications](./sdk.md#showing-notifications-error-and-success-messages).

```javascript
import Notification from '../utils/notification';

// Show error message (async/await)
await Notification.showErrorsMsg('Error message');

// Show success message (async/await)
await Notification.showSuccessMsg('Success message');
```

**Rules:**
- **ALWAYS** use Notification utility - Never create custom error/success message components
- **ALWAYS** show user-friendly error messages
- **NEVER** throw errors to the user
- Handle errors in services, not components
- Log errors to console for debugging
- The Notification utility uses Zoho's native UI - no HTML/components needed

---

## Code Organization Rules

### 1. Separation of Concerns

- **Services**: API calls, data transformation, business logic
- **Mixins**: Shared component logic
- **Components**: UI rendering, user interactions
- **Utils**: Helper functions, constants, utilities

### 2. No Code Duplication

- If code is used in 2+ components → Extract to mixin
- If API logic is duplicated → Extract to service
- If constants are repeated → Add to constants.js

### 3. Component Size

- View components should be < 250 lines
- If component exceeds 250 lines → Extract sub-components or mixins
- Services should be focused (one service per domain)

### 4. Naming Conventions

- **Services**: `*Service.js` (e.g., `bookingService.js`)
- **Mixins**: `*Mixin.js` (e.g., `paginationMixin.js`)
- **Components**: `PascalCase.vue` (e.g., `SearchText.vue`)
- **Constants**: `UPPER_SNAKE_CASE` (e.g., `MODAL_SIZE`)

### 5. Import Order

```javascript
// 1. Vue components
import ComponentA from '../components/ComponentA.vue';

// 2. Services
import MyService from '../services/myService';

// 3. Mixins
import myMixin from '../mixins/myMixin';

// 4. Utils
import { CONSTANT } from '../utils/constants';
import Notification from '../utils/notification';
```

---

## ZFAPPS SDK Integration

### Accessing ZFAPPS in Components

```javascript
// Access app metadata
const apiEndpoint = this.$appMeta.bookingApi;
const connection = this.$appMeta.bookingConnection;

// Access ZFAPPS methods
await window.ZFAPPS.get('organization');
await window.ZFAPPS.set('invoice.line_items', data);
window.ZFAPPS.closeModal();
window.ZFAPPS.invoke('RESIZE', { width: '920px', height: '750px' });
```

### Modal Widgets

For button/modal widgets:
- Use `widget_type: "modal"` in manifest (not "button")
- Resize modal using `ZFAPPS.invoke("RESIZE", ...)`
- Close modal using `ZFAPPS.closeModal()`

---

## Example: Creating a New Widget

### Step 1: Create Service

```javascript
// src/services/myWidgetService.js
import Notification from '../utils/notification';

class MyWidgetService {
  constructor(apiEndpoint, connectionName) {
    this.apiEndpoint = apiEndpoint;
    this.connectionName = connectionName;
  }

  async fetchWidgetData() {
    // Implementation
  }
}

export default MyWidgetService;
```

### Step 2: Create View Component

```javascript
// src/views/MyWidget.vue
<template>
  <!-- UI structure -->
</template>

<script>
import MyWidgetService from '../services/myWidgetService';
import paginationMixin from '../mixins/paginationMixin';

export default {
  name: 'MyWidget',
  mixins: [paginationMixin],
  data() {
    return {
      myWidgetService: null
    };
  },
  async created() {
    this.myWidgetService = new MyWidgetService(
      this.$appMeta.apiEndpoint,
      this.$appMeta.connectionName
    );
    // Initialize widget
  }
};
</script>
```

### Step 3: Add Route

```javascript
// src/router/index.js
import MyWidget from '../views/MyWidget.vue';

const routes = [
  {
    path: '/mywidget',
    name: 'mywidget',
    component: MyWidget
  }
];
```

---

## Checklist for New Widgets

- [ ] Service created in `src/services/` with proper error handling
- [ ] View component uses services (no direct API calls)
- [ ] Mixins used for shared functionality
- [ ] Constants imported from `utils/constants.js`
- [ ] No code duplication with existing widgets
- [ ] Component size < 250 lines
- [ ] JSDoc comments on all methods
- [ ] Error handling implemented
- [ ] Modal resizing handled (if applicable)
- [ ] Router route added (if new page)

---

## Common Patterns

### Fetching Data with Pagination

```javascript
async created() {
  this.service = new MyService(this.$appMeta.apiEndpoint, this.$appMeta.connectionName);
  const data = await this.service.fetchAllData();
  this.initializePagination(data);
  this.initializeSearch(this.itemArray);
  await this.createTable(this.itemArray);
}
```

### Handling User Actions

```javascript
async handleEvent() {
  this.isspin = true;
  this.isdisabled = true;
  
  const success = await this.service.performAction(this.selectedList);
  
  if (success) {
    window.ZFAPPS.closeModal();
  }
  
  this.isspin = false;
  this.isdisabled = false;
}
```

### Date Formatting

```javascript
formatDate(dateString) {
  const date = new Date(dateString);
  const monthNames = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 
                      'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'];
  const day = date.getDate();
  const month = monthNames[date.getMonth()];
  const year = date.getFullYear();
  return `${day < 10 ? '0' + day : day}-${month}-${year}`;
}
```

---

## Anti-Patterns (DO NOT)

❌ **DON'T** make API calls directly in components
```javascript
// BAD
async created() {
  const response = await window.ZFAPPS.request({...});
}

// GOOD
async created() {
  const data = await this.service.fetchData();
}
```

❌ **DON'T** duplicate code between components
```javascript
// BAD - Same method in multiple components
methods: {
  async fetchData() { /* duplicated code */ }
}

// GOOD - Use service or mixin
```

❌ **DON'T** use hardcoded strings
```javascript
// BAD
connection_link_name: 'bookings_finance_connection'

// GOOD
import { CONNECTION_NAMES } from '../utils/constants';
connection_link_name: CONNECTION_NAMES.FINANCE
```

❌ **DON'T** create large components (> 250 lines)
```javascript
// BAD - 500 line component
// GOOD - Extract to services, mixins, or sub-components
```

❌ **DON'T** create separate components for error/success messages
```javascript
// BAD - Creating custom error component
<template>
  <div class="error-message" v-if="error">
    {{ error }}
  </div>
</template>

// GOOD - Use Notification utility
import Notification from '../utils/notification';
const Notification = {
	show: (type, message) => {
		window.ZFAPPS.invoke('SHOW_NOTIFICATION', {
		  type,
		  message
		}).catch(err => {
		  console.error('Notification Error:', err);
		});
	  },
	  showSuccessMsg: (message) => {
		Notification.show('success', message);
	  },
	  showErrorsMsg: (message) => {
		Notification.show('error', message);
	  }
}
export default Notification
```

---

## Important Rules Summary

### ✅ DO:
- **Use existing Vue folder if it exists** - Overwrite/update code in it
- **Create Vue folder using CLI if it doesn't exist**: `vue create <folder_name>`
- **Write ALL Vue code in Vue folder** (`samplevueapp/src/`)
- **Include Zoho SDK script in index.html** before Vue app script
- **Set base path to "."** in vue.config.js or vite.config.js
- **Use default dist folder** - Do NOT set `outputDir` or `outDir` in build config
- **Keep widget folder clean** (only HTML, manifest, and built files)
- **Use localhost URL in plugin-manifest.json** when widgets folder exists (e.g., `https://localhost:8080#/route`)
- **Only update plugin-manifest.json** - Do NOT edit widget.html or other widget folder files
- **Check for existing widget folders first** - Search for `plugin-manifest.json` before creating new folder
- **Ask user before creating widget folder** - Never create widget folder without user confirmation

### ❌ DO NOT:
- Create webpack config in widget folder
- Write Vue code in widget folder
- Mix Vue code with widget folder structure
- Use webpack in widget folder for Vue code
- Write in widget folder when Vue framework is mentioned
- **Set `outputDir` or `outDir`** - Use default `dist` folder location
- **Edit widget.html or other widget folder files** - Only change plugin-manifest.json URL
- **Create widget folder without checking first** - Always search for existing `plugin-manifest.json`
- **Create widget folder without asking user** - Always ask before creating new widget folder

### Decision Tree:
```
Is Vue framework mentioned?
├─ YES → Does Vue folder exist?
│   ├─ YES → Use existing folder, overwrite/update code in it
│   └─ NO → Create folder using `vue create <folder_name>`, then write code
│
└─ NO (Pure JS/Vanilla JS) → Write directly in widget folder
```

---

## References

- [Vue.js 2 Documentation](https://v2.vuejs.org/)
- [Vue Router Documentation](https://router.vuejs.org/)
- [Zoho Finance SDK Rules](./sdk.md)
- [Widget Location Rules](./location.md)
- [Zoho Finance SDK](https://js.zohostatic.com/zohofinance/v1/zf_sdk.js)
