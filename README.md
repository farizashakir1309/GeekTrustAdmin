# Admin User Interface for User Management (Vue 2)
This project aims to create an intuitive and efficient user interface for administrators to manage users within the system. The UI is built using Vue.js 2 and interacts with the provided Users API to fetch user data. Users can be viewed, edited, and deleted within the UI, with support for sorting, searching, pagination, and batch deletion.

## Table of Contents
- [Link](#link)
- [Requirements](#requirements)
- [Getting Started](#getting-started)
- [Features](#features)
- [API Usage](#api-usage)

## Link
https://csb-4pkfkr.netlify.app/
<img width="946" alt="image" src="https://github.com/farizashakir1309/geek-trust-admin-ui-challenge/assets/69107931/e67d9435-3432-42ad-b6dc-5ee46e3686d4">

## Requirements
1. The column titles must be distinguishable from the user entries for better readability.
2. A search bar is available for users to filter based on any property.
3. Rows can be edited or deleted directly (in-memory only, no persistence).
4. Pagination with 10 rows per page and navigation buttons for first, previous, next, and last pages.
5. Selection of one or more rows with gray background highlight. Multiple selected rows can be deleted together.
6. A checkbox at the top-left corner allows for selecting/deselecting all displayed rows on the current page.

## Getting Started
Follow these steps to set up and run the project locally:

1. Clone this repository.
2. Install dependencies using `npm install`.
3. Run the development server using `npm run serve`.

## Features
1. **Column Titles:** The table displays user data with distinct column titles for easy identification.
2. **Search Bar:** A search bar allows users to filter the displayed records based on any property, making it easier to find specific users.
3. **Edit and Delete:** Users can edit and delete rows directly within the table. These actions are in-memory only and do not persist to any backend.
4. **Pagination:** Pagination is implemented with 10 rows per page. Navigation buttons enable users to move through pages, adjusting based on filtering.
5. **Row Selection:** Rows can be individually selected. Selected rows are highlighted with a gray background. A checkbox at the top-left corner selects/deselects all visible rows on the current page.
6. **Delete Selected:** Users can delete multiple selected rows at once using the "Delete Selected" button at the bottom-left corner.

## API Usage
This project uses the Users API to retrieve user data for display and interaction. The API endpoint is [https://geektrust.s3-ap-southeast-1.amazonaws.com/adminui-problem/members.json](https://geektrust.s3-ap-southeast-1.amazonaws.com/adminui-problem/members.json). The API provides a list of user objects with properties like `id`, `name`, `email`, and `role`.

Sample API Response:

```json
[
  {
    "id": "1",
    "name": "Aaron Miles",
    "email": "aaron@mailinator.com",
    "role": "member"
  },
  {
    "id": "2",
    "name": "Aishwarya Naik",
    "email": "aishwarya@mailinator.com",
    "role": "member"
  },
  {
    "id": "3",
    "name": "Arvind Kumar",
    "email": "arvind@mailinator.com",
    "role": "admin"
  }
  // ...
]
