<template>
  <v-container>
    <v-row no-gutters>
      <v-col class="mx-auto">
        <v-card class="pa-5">
          <v-card-title>Add new post</v-card-title>
          <v-divider></v-divider>
          <v-form
            class="pa-5"
            @submit.prevent="submitForm"
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
              :rules="rules"
              show-size
              counter
              multiple
              label="Select Image"
            ></v-file-input>

            <v-btn class="mt-3" type="submit" color="primary">Add Post</v-btn>
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
  methods: {
    selectFile(file) {
      this.image = file[0];
    },
    async submitForm() {
      const formData = new FormData();
      formData.append('image', this.image);
      formData.append('title', this.post.title);
      formData.append('category', this.post.category);
      formData.append('content', this.post.content);
      if (this.$refs.form.validate()) {
        const response = await API.addPost(formData);
        console.log(response)
        this.$router.push({
          name: 'home',
          params: { message: response.message },
        });
      }
    },
  },
};
</script>
