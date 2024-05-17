<script>
import axios from "axios";
import ShowUser from "@/components/admin/ShowUser.vue";

export default {
  components: { ShowUser },

  data() {
    return {
      users: {},
    };
  },

  async created() {
    try {
      const users = await axios({
        baseURL: import.meta.env.VITE_BACKENDURL,
        method: "get",
        url: "/admin/user/list",
      });
      this.users = users.data;
      // console.log(this.users);
    } catch (e) {
      console.log(e);
    }
  },

  methods: {
    async deleteUser(userID) {
      try {
        await axios({
          baseURL: import.meta.env.VITE_BACKENDURL,
          method: "delete",
          url: "/admin/user/delete",
          params: {
            userID: userID,
          },
        });

        window.location.reload();
      } catch (e) {
        console.log(e);
      }
    },
  },
};
</script>

<template>
  <main>
    <div class="w-full p-8">
      <div
        class="flex flex-col space-y-2 sm:flex-row sm:space-y-0 sm:justify-between"
      >
        <h2 class="font-bold text-2xl">Users</h2>
      </div>
      <div class="divider"></div>
      <div class="overflow-x-auto">
        <table class="table w-full">
          <!-- head -->
          <thead>
            <tr>
              <th></th>
              <th>Avatar</th>
              <th>Username</th>
              <th>Role</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr class="hover" v-for="(user, key) in this.users" :key="key">
              <td>{{ key + 1 }}</td>
              <td>
                <div class="avatar">
                  <div class="w-16 rounded">
                    <img :src="user.avatar" alt="user profile" />
                  </div>
                </div>
              </td>
              <td>{{ user.username }}</td>
              <td>{{ user.role.name }}</td>
              <td class="flex space-x-2">
                <label :for="'deleteUser' + key" class="btn btn-error"
                  >Delete</label
                >
                <ShowUser :userID="user._id" :userKey="key"></ShowUser>
              </td>
              <input
                type="checkbox"
                :id="'deleteUser' + key"
                class="modal-toggle"
              />
              <div class="modal">
                <div class="modal-box relative">
                  <label
                    :for="'deleteUser' + key"
                    class="btn btn-sm btn-circle absolute right-2 top-2"
                    >✕</label
                  >
                  <h3 class="font-bold text-lg">
                    Are you sure you want to delete this user - "{{
                      user.username
                    }}"?
                  </h3>
                  <div class="divider"></div>
                  <div class="flex space-x-4">
                    <button
                      @click="deleteUser(user._id)"
                      class="btn btn-error btn-outline"
                    >
                      Delete
                    </button>
                    <label :for="'deleteUser' + key" class="btn btn-primary"
                      >Cancel</label
                    >
                  </div>
                </div>
              </div>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </main>
</template>
