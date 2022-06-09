<template>
  <v-container>
    <v-row no-gutters>
      <v-col class="mx-auto">
        <v-card class="pa-5">
          <v-card-title>Edit post</v-card-title>
          <v-divider></v-divider>
          <v-form
            class="pa-5"
            @submit.prevent="updateForm"
            ref="form"
            enctype="multipart/form-data"
          >
            <v-text-field
              v-model="post.title"
              label="Title"
              prepend-icon="mdi-note"
              :rules="rules"
            ></v-text-field>
            <v-text-field
              v-model="post.category"
              label="Category"
              prepend-icon="mdi-view-list"
              :rules="rules"
            ></v-text-field>
            <v-textarea
              v-model="post.content"
              label="Content"
              prepend-icon="mdi-note-plus"
              :rules="rules"
            ></v-textarea>

            <v-file-input
              @change="selectFile"
              show-size
              counter
              multiple
              label="Select Image"
            ></v-file-input>

            <v-img :src="`/${post.image}`" width="120"></v-img>

            <v-btn class="mt-3" type="submit" color="success"
              >Update Post</v-btn
            >
          </v-form>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import API from '../api';
export default {
  data() {
    return {
      rules: [(value) => !!value || 'This field is required'],
      post: {
        title: '',
        category: '',
        content: '',
        image: '',
      },
      image: '',
    };
  },
  async created() {
    const response = await API.getPostByID(this.$route.params.id);
    this.post = response;
  },
  methods: {
    selectFile(file) {
      this.image = file[0];
    },
    async updateForm() {
      const formData = new FormData();
      formData.append('image', this.image);
      formData.append('title', this.post.title);
      formData.append('category', this.post.category);
      formData.append('content', this.post.content);
      formData.append('old_image', this.post.image);
      if (this.$refs.form.validate()) {
        const response = await API.updatePost(this.$route.params.id, formData);
        console.log(response);
        this.$router.push({
          name: 'home',
          params: { message: response.message },
        });
      }
    },
  },
};
</script>
