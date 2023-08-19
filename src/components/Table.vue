<template>
  <div class="admin-dashboard">
    <link
      rel="stylesheet"
      href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.3/css/all.min.css"
    />
    <SearchBar @search="handleSearch" />
    <table class="user-table">
      <thead>
        <tr>
          <th><input type="checkbox" @click="selectAll" /></th>
          <th>Name</th>
          <th>Email</th>
          <th>Role</th>
          <th>Actions</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(user, key) in getPaginatedUsers"
          :key="key"
          :id="`user-id-${user.id}`"
        >
          <td>
            <input
              type="checkbox"
              @click="updateSelectedUser(user)"
              value="user.id"
            />
          </td>
          <td>
            <span v-if="editingIndex !== user.id">{{ user.name }}</span>
            <input v-else v-model="user.name" @blur="saveEditedUser(user.id)" />
          </td>
          <td v-text="user.email"></td>
          <td>
            <span v-if="editingIndex !== user.id">{{ user.role }}</span>
            <select v-else v-model="user.role" @blur="saveEditedUser(user.id)">
              <option value="member">Member</option>
              <option value="admin">Admin</option>
            </select>
          </td>
          <td>
            <button @click="editUser(user)">
              <i class="fas fa-regular fa-edit" style="color: #0a0a0a"></i>
              <!-- FontAwesome edit icon -->
            </button>
            <button @click="deleteUser(user, key)">
              <i class="fas fa-regular fa-trash" style="color: #be1604"></i>
            </button>
          </td>
        </tr>
      </tbody>
    </table>
    <button class="delete-selected" @click="deleteSelected">
      Delete Selected
    </button>
    <Pagination
      :current-page="currentPage"
      :total-pages="totalPages"
      @page-change="handlePagination"
    />
  </div>
</template>
<script>
import SearchBar from "./SearchBar";
import Pagination from "./Pagination";
import { getUserData } from "../user";
export default {
  name: "Table",
  components: {
    SearchBar,
    Pagination,
  },

  data() {
    return {
      users: null,
      selectedUser: new Map(),
      editingIndex: -1,
      searchQuery: "",
      currentPage: 1,
      itemsPerPage: 10,
    };
  },
  created() {
    getUserData().then((users) => {
      this.users = users;
    });
  },
  computed: {
    getFilteredUsers() {
      // Return the users that match the search query

      if (this.searchQuery.trim() === "") {
        return this.users; // Return paginated users if no search query
      }
      const lowerCaseQuery = this.searchQuery.toLowerCase();
      const filtered = this.users.filter((user) => {
        return (
          user.name.toLowerCase().includes(lowerCaseQuery) ||
          user.email.toLowerCase().includes(lowerCaseQuery) ||
          user.role.toLowerCase().includes(lowerCaseQuery)
        );
      });
      return filtered;
    },
    getPaginatedUsers() {
      if (this.getFilteredUsers === null) {
        return this.users;
      }
      const startIndex = (this.currentPage - 1) * this.itemsPerPage;
      return this.getFilteredUsers.slice(
        startIndex,
        startIndex + this.itemsPerPage
      );
    },
    totalPages() {
      if (this.getFilteredUsers === null) {
        return 25;
      }
      return Math.ceil(this.getFilteredUsers.length / this.itemsPerPage);
    },
  },
  methods: {
    updateSelectedUser(user) {
      const userId = document.querySelector(`#user-id-${user.id}`);
      if (this.selectedUser.has(user.id)) {
        this.selectedUser.delete(user.id);
        userId.classList.remove("selected");
      } else {
        this.selectedUser.set(user.id, user);
        userId.classList.add("selected");
      }
      console.log(this.selectedUser);
    },
    deleteUser(user, key) {
      let users = this.users;
      const userId = document.querySelector(`#user-id-${user.id}`);
      users.splice(key, 1);
      userId.remove();
      if (this.selectedUser.has(user.id)) {
        this.selectedUser.delete(user.id);
      }
      this.users = users;
    },
    editUser(user) {
      this.editingIndex = user.id;
    },
    saveEditedUser() {
      this.editingIndex = -1;
    },
    handleSearch(query) {
      this.searchQuery = query;

      // Clear selected users when performing a search
      this.selectedUser.clear();
    },
    handlePagination(page) {
      this.currentPage = page;
    },
    selectAll() {
      this.selectedUser.clear(); // Clear existing selections
      this.getFilteredUsers.forEach((user) => {
        this.selectedUser.set(user.id, user);
        const userId = document.querySelector(`#user-id-${user.id}`);
        if (userId) {
          const checkbox = userId.querySelector("input[type='checkbox']");
          checkbox.checked = true;
          userId.classList.add("selected");
        }
      });
    },
    deleteSelected() {
      const selectedIds = Array.from(this.selectedUser.keys());
      this.users = this.users.filter((user) => {
        if (selectedIds.includes(user.id)) {
          const userId = document.querySelector(`#user-id-${user.id}`);
          if (userId) {
            userId.remove();
          }
          return false; // Exclude selected user from the new users array
        }
        return true; // Keep non-selected users in the new array
      });

      this.selectedUser.clear(); // Clear selections after deletion
    },
  },
};
</script>
<style scoped>
.admin-dashboard {
  display: block;
  margin: 40px;
  font-family: Arial, sans-serif;
}
table {
  width: 100%;
  overflow-x: auto; /* Enable horizontal scrolling on small screens */
  object-fit: contain;
  border-collapse: collapse;
  margin-bottom: 10px;
}
th,
td {
  padding: 10px;
  text-align: left;
  border-bottom: 1px solid #ccc;
}
th {
  font-weight: 600;
}
tr:hover {
  background-color: #ddd;
}
tr.selected {
  background: #ddd;
}
input[type="checkbox"] {
  margin: 0;
  cursor: pointer;
  width: 15px;
  height: 15px;
  border: 1px solide black;
}

.user-table button {
  padding: 6px 10px;
  background-color: transparent;
  color: white;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}
.user-table button + button {
  margin-left: 10px;
}
.delete-selected {
  color: white;
  background-color: #be1604;
  float: left;
  border-radius: 20px;
  padding: 4px 8px 4px 8px;
  display: flex;
  align-items: center;
}
</style>