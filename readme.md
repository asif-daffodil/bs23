# 1. Database Architecture Design

Designing a database for an e-commerce platform involves considering various aspects such as user profiles, product listings, categories, orders, and transaction history. The choice between a Relational Database Management System (RDBMS) or a NoSQL database depends on the specific requirements of the platform.

### Database System Choice:

For an e-commerce platform with structured data and complex relationships (users, products, orders), an RDBMS like PostgreSQL, MySQL, or Microsoft SQL Server might be suitable. If there is a need for flexibility in data models or scalability, a NoSQL solution like MongoDB could be considered.

### Database Architecture:

1. **User Profiles:**
    - **Table/Collection:** `Users`
    - **Fields:**
        - UserID (Primary Key)
        - Username
        - Email
        - Password (Hashed)
        - Address
        - Phone Number
        - Other relevant user information
2. **Product Listings:**
    - **Table/Collection:** `Products`
    - **Fields:**
        - ProductID (Primary Key)
        - SellerID (Foreign Key referencing Users)
        - Name
        - Description
        - Price
        - Quantity
        - CategoryID (Foreign Key referencing Categories)
        - Other product details
3. **Product Categories:**
    - **Table/Collection:** `Categories`
    - **Fields:**
        - CategoryID (Primary Key)
        - CategoryName
4. **Orders:**
    - **Table/Collection:** `Orders`
    - **Fields:**
        - OrderID (Primary Key)
        - UserID (Foreign Key referencing Users)
        - OrderDate
        - TotalAmount
        - Status (e.g., Pending, Shipped, Delivered)
5. **Transaction History:**
    - **Table/Collection:** `Transactions`
    - **Fields:**
        - TransactionID (Primary Key)
        - OrderID (Foreign Key referencing Orders)
        - TransactionDate
        - PaymentMethod
        - Amount

### Relationships:

- Users and Products: One-to-Many relationship (One user can have many products listed)
- Products and Categories: Many-to-One relationship (Many products can belong to one category)
- Users and Orders: One-to-Many relationship (One user can have many orders)
- Orders and Transactions: One-to-One relationship (One order has one transaction)

### Scalability and Performance:

- Use indexing on key columns to speed up search and retrieval.
- Consider denormalization for frequently accessed data to reduce complex joins.
- Implement caching mechanisms to reduce database load.
- Scale vertically (upgrading hardware) or horizontally (sharding) based on performance needs.

### Security Measures:

- **Encryption:** Use SSL/TLS for data in transit and encryption for sensitive fields.
- **Hashed Passwords:** Store passwords securely using strong cryptographic hashing algorithms.
- **Authentication and Authorization:** Implement secure authentication mechanisms, and ensure that users can only access data they are authorized to.
- **Parameterized Queries:** Use parameterized queries or prepared statements to prevent SQL injection attacks.
- **Regular Security Audits:** Regularly audit and update security protocols to address emerging threats.


# 2. Coding Problem

Here's a PHP function named `filterProducts` that meets the requirements:

```php
<?php

function filterProducts($products, $categoryName) {
    $filteredProducts = [];

    foreach ($products as $product) {
        $productCategory = strtolower($product['category']);
        $searchCategory = strtolower($categoryName);

        // Check if the category matches exactly or contains the specified category name
        if ($productCategory === $searchCategory || strpos($productCategory, $searchCategory) !== false) {
            $filteredProducts[] = $product;
        }
    }

    return $filteredProducts;
}

// Sample products array
$products = [
    ['name' => 'Product 1', 'price' => 25.99, 'category' => 'Backend Development'],
    ['name' => 'Product 2', 'price' => 49.99, 'category' => 'Frontend Development'],
    ['name' => 'Product 3', 'price' => 12.99, 'category' => 'Full Stack Development'],
    ['name' => 'Product 4', 'price' => 34.99, 'category' => 'Mobile App Development'],
];

// Sample cases
$categoryName1 = 'backend';
$categoryName2 = 'development';

$filteredProducts1 = filterProducts($products, $categoryName1);
$filteredProducts2 = filterProducts($products, $categoryName2);

// Displaying the results
echo "Products in the category '{$categoryName1}':\\n";
print_r($filteredProducts1);

echo "\\nProducts in the category '{$categoryName2}':\\n";
print_r($filteredProducts2);
?>

```

This function takes an array of products and a category name as input, and it returns a new array containing only the products that match the specified category or contain the category name. The sample cases demonstrate how to use the function with different category names.


# 3. Designing RESTful API for User Management

Designing a robust and scalable RESTful API for managing user profiles in a high-traffic application requires careful planning and consideration of various factors. Here's an approach addressing the key aspects:

### URL Structure and HTTP Methods:

1. **Create (POST):**
    - Endpoint: `/api/user-profiles`
    - Use POST method to create new user profiles.
2. **Read (GET):**
    - Endpoint: `/api/user-profiles/{userID}`
    - Use GET method to retrieve specific user profile by userID.
3. **Update (PUT/PATCH):**
    - Endpoint: `/api/user-profiles/{userID}`
    - Use PUT/PATCH method to update specific user profile by userID.
4. **Delete (DELETE):**
    - Endpoint: `/api/user-profiles/{userID}`
    - Use DELETE method to remove a specific user profile by userID.

### Data Format for Requests and Responses:

- Use JSON as the data format for both requests and responses due to its simplicity and readability.

### Authentication and Authorization:

- Implement token-based authentication (JWT) to secure endpoints.
- Use OAuth 2.0 for handling authorization to restrict access to specific operations based on user roles.

### Scalability and Security:

- Employ horizontal scaling by distributing load across multiple servers using load balancers.
- Implement rate limiting to prevent abuse and ensure fair usage.
- Use HTTPS to encrypt data transmission.
- Employ security best practices like input validation, parameterized queries, and proper error handling.

### Middleware/Frameworks:

- **Node.js with Express.js** can be used to build the API due to its scalability and performance.
- Utilize middleware such as `body-parser` for parsing JSON, `helmet` for security headers, `cors` for cross-origin resource sharing, and `jsonwebtoken` for JWT authentication.

### Integration with Frontend Framework:

- For integration with frontend frameworks like Angular or Vue.js:
    - Use HTTP requests (GET, POST, PUT, DELETE) from frontend components to interact with API endpoints.
    - Implement JWT authentication in frontend by storing tokens securely (e.g., in local storage) and sending them in headers for API requests.
    - Handle responses and update the UI accordingly using components and state management libraries (e.g., Vuex for Vue.js or NgRx for Angular).

Bonus (Frontend Integration Example):

```jsx
// Vue.js Example - Using Axios for HTTP requests and Vuex for state management

// UserService.js - Manage API calls
import axios from 'axios';

const API_BASE_URL = '<http://your-api-base-url/api/user-profiles>';

export default {
  getUserProfile(userID) {
    return axios.get(`${API_BASE_URL}/${userID}`);
  },
  createUserProfile(profileData) {
    return axios.post(API_BASE_URL, profileData);
  },
  updateUserProfile(userID, updatedData) {
    return axios.put(`${API_BASE_URL}/${userID}`, updatedData);
  },
  deleteUserProfile(userID) {
    return axios.delete(`${API_BASE_URL}/${userID}`);
  }
};

// UserProfile.vue - Example component
<template>
  <div>
    <!-- Display User Profile Data -->
    <div v-if="userProfile">
      <h2>{{ userProfile.name }}</h2>
      <p>Email: {{ userProfile.email }}</p>
      <!-- Other profile fields -->
    </div>
    <!-- Update Profile Form -->
    <form v-if="editing" @submit.prevent="updateProfile">
      <!-- Form fields for updating profile -->
      <!-- Example: <input v-model="updatedProfile.name"> -->
      <button type="submit">Update Profile</button>
    </form>
    <!-- Button to toggle edit mode -->
    <button @click="toggleEditMode">{{ editing ? 'Cancel' : 'Edit' }}</button>
  </div>
</template>

<script>
import UserService from './UserService';

export default {
  data() {
    return {
      userProfile: null,
      updatedProfile: {},
      editing: false
    };
  },
  created() {
    this.fetchUserProfile();
  },
  methods: {
    fetchUserProfile() {
      const userID = 'user-id-to-fetch'; // Replace with actual user ID
      UserService.getUserProfile(userID)
        .then(response => {
          this.userProfile = response.data;
          this.updatedProfile = { ...this.userProfile }; // Clone for editing
        })
        .catch(error => {
          console.error('Error fetching user profile:', error);
        });
    },
    toggleEditMode() {
      this.editing = !this.editing;
    },
    updateProfile() {
      const userID = 'user-id-to-update'; // Replace with actual user ID
      UserService.updateUserProfile(userID, this.updatedProfile)
        .then(response => {
          this.userProfile = response.data;
          this.editing = false;
        })
        .catch(error => {
          console.error('Error updating user profile:', error);
        });
    }
  }
};
</script>

```

This example demonstrates a Vue.js component for managing user profiles, utilizing Axios for API requests and Vuex for managing state related to user profiles.
